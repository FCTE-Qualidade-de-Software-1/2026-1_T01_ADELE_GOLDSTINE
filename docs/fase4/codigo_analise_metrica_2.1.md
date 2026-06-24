# Codigo para métrica 2.1 - Percentual de operações críticas com tratamento de exceção

````markdown
"""
M2.1 — Percentual de operações críticas com tratamento de exceção
Categorias: Autenticação e Segurança | Regras de Negócio e BD | Infraestrutura e Integração | Web Scraping
"""

import ast
import os
import re
import json
from pathlib import Path
from dataclasses import dataclass, field
from typing import Optional
from datetime import datetime

ROOT = Path(**file**).parent.parent
BACKEND_ROOT = ROOT / "api"
FRONTEND_ROOT = ROOT / "web"

EXCLUDE_DIRS = {"migrations", "tests", "**pycache**"}
EXCLUDE_FILES = {"manage.py"} # settings ficam incluídos pois carregam credenciais

# ---------------------------------------------------------------------------

# Categorias e padrões de operações críticas

# A ordem importa: primeira categoria que bater numa linha vence.

# ---------------------------------------------------------------------------

# Cada entrada: (regex, categoria)

# Prioridade descendente: auth_security > webscraping > business_db > infra_integration

PATTERNS*PY: list[tuple[str, str]] = [ # ── Autenticação e Segurança ────────────────────────────────────────── # JWT: validação de token
(r"serializer\.is_valid\s*\(", "auth_security"),
(r"serializer\.get_token\s*\(", "auth_security"),
(r"request\.COOKIES\s*\[", "auth_security"),
(r"response\.set_cookie\s*\(", "auth_security"),
(r"response\.data\.pop\s*\(", "auth_security"), # Google OAuth2: chamada HTTP para obter dados do usuário
(r"requests\.get\s*\(", "auth_security"),
(r"response\.json\s*\(\)", "auth_security"), # Funções de autenticação
(r"get_user_data\s*\(", "auth_security"),
(r"do_auth\s*\(", "auth_security"),
(r"get_backend\s*\(", "auth_security"),
(r"\bauthenticate\s*\(", "auth_security"),
(r"\blogin\s*\(", "auth_security"),
(r"\blogout\s*\(", "auth_security"),
(r"check_permission_to_save\s*\(", "auth_security"),
(r"request\.user\b", "auth_security"), # ORM específico de autenticação (modelo User e operações de acesso privilegiado)
(r"User\.objects\.(create_superuser|get_or_create|all|filter|create|get)\b", "auth_security"),
(r"admin\.save\s*\(", "auth_security"), # Leitura de segredos e credenciais (SECRET_KEY, OAUTH tokens, credenciais de admin)
(r'config\s*\(\s*["\']DJANGO_SECRET_KEY', "auth_security"),
(r'config\s*\(\s*["\']GOOGLE_OAUTH2', "auth_security"),
(r'config\s*\(\s\*["\']ADMIN*', "auth_security"),

    # ── Web Scraping SIGAA ────────────────────────────────────────────────
    (r"BeautifulSoup\s*\(", "webscraping"),
    (r"create_request_session\s*\(", "webscraping"),
    (r"get_session_cookie\s*\(", "webscraping"),
    (r"get_response\s*\(", "webscraping"),
    (r"self\.session\.(get|post)\s*\(", "webscraping"),
    (r"session\.(get|post)\s*\(", "webscraping"),
    (r"get_response_from_disciplines_post_request\s*\(", "webscraping"),
    (r"make_web_scraping_of_disciplines\s*\(", "webscraping"),
    (r"get_disciplines\s*\(", "webscraping"),
    (r"get_list_of_departments\s*\(", "webscraping"),
    (r"response\.content\b", "webscraping"),
    (r"soup\.find_all\s*\(", "webscraping"),
    (r"soup\.find\s*\(", "webscraping"),
    (r"DisciplineWebScraper\s*\(", "webscraping"),

    # ── Regras de Negócio e Banco de Dados ───────────────────────────────
    # ORM geral (exceto User que é capturado acima em auth_security)
    (r"\.objects\.(get|create|filter|all|get_or_create|update|delete|bulk_create|bulk_update|first|last|exists|count)\b", "business_db"),
    (r"\.(save|delete)\s*\(", "business_db"),
    # Funções de negócio (schedule, search, etc.)
    (r"save_schedule\s*\(", "business_db"),
    (r"get_schedules\s*\(", "business_db"),
    (r"delete_schedule\s*\(", "business_db"),
    (r"get_or_create_department\s*\(", "business_db"),
    (r"get_or_create_discipline\s*\(", "business_db"),
    (r"create_class\s*\(", "business_db"),
    (r"get_class_by_", "business_db"),
    (r"filter_disciplines_by_", "business_db"),
    (r"get_best_similarities", "business_db"),
    (r"generate_schedule\s*\(", "business_db"),
    (r"serializer\.validated_data\b", "business_db"),

    # ── Infraestrutura e Integração ───────────────────────────────────────
    # Arquivos
    (r"\bopen\s*\(", "infra_integration"),
    (r"os\.(remove|unlink|rename|makedirs|mkdir)\s*\(", "infra_integration"),
    (r"\.(read|write|readline|readlines)\s*\(", "infra_integration"),
    # Cache
    (r"cache\.(delete|get|set|clear)\s*\(", "infra_integration"),
    # Configuração de infraestrutura (DB, Redis, paths de settings)
    (r'config\s*\(\s*["\']DB_', "infra_integration"),
    (r'config\s*\(\s*["\']REDIS_', "infra_integration"),
    (r'config\s*\(\s*["\']SETTINGS_FILE_PATH', "infra_integration"),
    (r"dj_database_url\.config\s*\(", "infra_integration"),
    (r"os\.environ\.(setdefault|get)\s*\(", "infra_integration"),
    # Setup de sessão HTTP (cliente de rede)
    (r"HTTPAdapter\s*\(", "infra_integration"),
    (r"Retry\s*\(", "infra_integration"),
    (r"session\.mount\s*\(", "infra_integration"),
    (r"\bSession\s*\(", "infra_integration"),

]

PATTERNS_TS: list[tuple[str, str]] = [ # ── Autenticação e Segurança ──────────────────────────────────────────
(r"\bsignInWithGoogle\s*\(", "auth_security"),
(r"\bregisterWithGoogle\s*\(", "auth_security"),
(r"\bretrieveAccessToken\s*\(", "auth_security"),
(r"\bsignOut\s*\(", "auth_security"),
(r"\blogout\s*\(", "auth_security"),
(r"\bsetAccessToken\s*\(", "auth_security"),

    # ── Web Scraping SIGAA ────────────────────────────────────────────────
    # (Frontend não faz scraping direto; categoria vazia para TypeScript)

    # ── Regras de Negócio e Banco de Dados ───────────────────────────────
    (r"\bgetSchedules\s*\(", "business_db"),
    (r"\bsaveSchedule\s*\(", "business_db"),
    (r"\bdeleteSchedule\s*\(", "business_db"),
    (r"\bgenerateSchedule\s*\(", "business_db"),
    (r"\bsearchDiscipline\s*\(", "business_db"),
    (r"\bretrieveYearAndPeriod\s*\(", "business_db"),

    # ── Infraestrutura e Integração ───────────────────────────────────────
    (r"localStorage\.(getItem|setItem|removeItem)\s*\(", "infra_integration"),
    (r"\bJSON\.parse\s*\(", "infra_integration"),
    (r"\brequest\.(get|post|put|delete|patch)\s*\(", "infra_integration"),
    (r"\baxios\.create\s*\(", "infra_integration"),

]

CATEGORIES = {
"auth_security": "Autenticação e Segurança",
"webscraping": "Web Scraping SIGAA",
"business_db": "Regras de Negócio e Banco de Dados",
"infra_integration": "Infraestrutura e Integração",
}

# ---------------------------------------------------------------------------

# Data structures

# ---------------------------------------------------------------------------

@dataclass
class Occurrence:
file: str
line: int
category: str
pattern_matched: str
code_snippet: str
has_exception_handling: bool
false_positive: bool = False
note: str = ""

# ---------------------------------------------------------------------------

# AST visitor — Python

# ---------------------------------------------------------------------------

class CriticalOpVisitor(ast.NodeVisitor):
"""Visita nós de chamada/atributo e registra se estão dentro de try/except."""

    def __init__(self, source_lines: list[str]):
        self.source_lines = source_lines
        self.occurrences: list[dict] = []
        self._try_depth = 0
        self._seen: set[tuple[int, str]] = set()   # (lineno, category) para deduplicar

    def visit_Try(self, node: ast.Try):
        self._try_depth += 1
        self.generic_visit(node)
        self._try_depth -= 1

    def visit_TryStar(self, node):
        self._try_depth += 1
        self.generic_visit(node)
        self._try_depth -= 1

    def _get_snippet(self, lineno: int) -> str:
        if 1 <= lineno <= len(self.source_lines):
            return self.source_lines[lineno - 1].strip()
        return ""

    def _record(self, call_str: str, lineno: int):
        for pat, cat in PATTERNS_PY:
            if re.search(pat, call_str):
                key = (lineno, cat)
                if key not in self._seen:
                    self._seen.add(key)
                    self.occurrences.append({
                        "line": lineno,
                        "category": cat,
                        "pattern": pat,
                        "snippet": self._get_snippet(lineno),
                        "in_try": self._try_depth > 0,
                    })
                return  # first-match wins

    def visit_Call(self, node: ast.Call):
        try:
            call_str = ast.unparse(node)
        except Exception:
            call_str = self._get_snippet(node.lineno)
        self._record(call_str, node.lineno)
        self.generic_visit(node)

    def visit_Attribute(self, node: ast.Attribute):
        # Captura acessos como request.user, response.content sem parênteses
        try:
            attr_str = ast.unparse(node)
        except Exception:
            attr_str = ""
        # Só registrar atributos sem chamada (os com chamada já são capturados como Call)
        if not isinstance(node.ctx, ast.Load):
            self.generic_visit(node)
            return
        if not hasattr(node, '_parent_is_call'):
            self._record(attr_str, node.lineno)
        self.generic_visit(node)

    def visit_Subscript(self, node: ast.Subscript):
        # Captura request.COOKIES['refresh']
        try:
            sub_str = ast.unparse(node)
        except Exception:
            sub_str = ""
        self._record(sub_str, node.lineno)
        self.generic_visit(node)

# ---------------------------------------------------------------------------

# Helpers

# ---------------------------------------------------------------------------

def is_excluded(path: Path) -> bool:
for part in path.parts:
if part in EXCLUDE_DIRS:
return True
for excl in EXCLUDE_FILES:
if path.name == excl:
return True
return False

def collect_py_files(root: Path) -> list[Path]:
return sorted(p for p in root.rglob("\*.py") if not is_excluded(p))

def collect_ts_files(root: Path) -> list[Path]:
return sorted(
p for p in root.rglob("\*")
if p.suffix in (".ts", ".tsx") and not is_excluded(p)
)

# ---------------------------------------------------------------------------

# Análise Python via AST

# ---------------------------------------------------------------------------

def analyze_python_file(filepath: Path) -> list[Occurrence]:
source = filepath.read_text(encoding="utf-8", errors="replace")
lines = source.splitlines()

    try:
        tree = ast.parse(source, filename=str(filepath))
    except SyntaxError as e:
        print(f"  [WARN] SyntaxError em {filepath}: {e}")
        return []

    visitor = CriticalOpVisitor(lines)
    visitor.visit(tree)

    rel = str(filepath.relative_to(ROOT))
    return [
        Occurrence(
            file=rel,
            line=raw["line"],
            category=raw["category"],
            pattern_matched=raw["pattern"],
            code_snippet=raw["snippet"],
            has_exception_handling=raw["in_try"],
        )
        for raw in visitor.occurrences
    ]

# ---------------------------------------------------------------------------

# Análise TypeScript via grep linha-a-linha

# ---------------------------------------------------------------------------

def \_find_try_catch_ranges(lines: list[str]) -> list[tuple[int, int]]:
ranges = []
depth_stack: list[int] = []
brace_depth = 0
try_re = re.compile(r'\btry\s*\{')
catch_re = re.compile(r'\}\s*catch\b')

    for lineno_0, line in enumerate(lines):
        lineno = lineno_0 + 1
        brace_depth += line.count("{") - line.count("}")
        if try_re.search(line):
            depth_stack.append(lineno)
        if catch_re.search(line) and depth_stack:
            start = depth_stack.pop()
            ranges.append((start, lineno))

    return ranges

def \_line_in_try_range(lineno: int, ranges: list[tuple[int, int]]) -> bool:
return any(start <= lineno <= end for start, end in ranges)

def analyze_ts_file(filepath: Path) -> list[Occurrence]:
source = filepath.read_text(encoding="utf-8", errors="replace")
lines = source.splitlines()
rel = str(filepath.relative_to(ROOT))
try_ranges = \_find_try_catch_ranges(lines)
occurrences = []
seen: set[tuple[int, str]] = set()

    for lineno_0, line in enumerate(lines):
        lineno = lineno_0 + 1
        for pat, cat in PATTERNS_TS:
            if re.search(pat, line):
                key = (lineno, cat)
                if key not in seen:
                    seen.add(key)
                    occurrences.append(Occurrence(
                        file=rel,
                        line=lineno,
                        category=cat,
                        pattern_matched=pat,
                        code_snippet=line.strip(),
                        has_exception_handling=_line_in_try_range(lineno, try_ranges),
                    ))
                break  # first-match wins

    return occurrences

# ---------------------------------------------------------------------------

# Pós-processamento: falsos positivos

# ---------------------------------------------------------------------------

\_FP_RE = [re.compile(p) for p in [
r"^\s*(import|from|export)\s",
r"^\s*#",
r"^\s*//",
r"^\s*\*",
]]

def mark_false_positives(occurrences: list[Occurrence]) -> list[Occurrence]:
for occ in occurrences:
for pat in \_FP_RE:
if pat.search(occ.code_snippet):
occ.false_positive = True
occ.note = "import/export/comment"
break
return occurrences

# ---------------------------------------------------------------------------

# Relatório

# ---------------------------------------------------------------------------

def build_report(all_occs: list[Occurrence]) -> dict:
valid = [o for o in all_occs if not o.false_positive]
covered = [o for o in valid if o.has_exception_handling]
total = len(valid)
pct = (len(covered) / total \* 100) if total else 0.0

    by_category: dict[str, dict] = {}
    for cat_key, cat_label in CATEGORIES.items():
        cat_occs = [o for o in valid if o.category == cat_key]
        cat_cov = [o for o in cat_occs if o.has_exception_handling]
        if cat_occs:
            by_category[cat_key] = {
                "label": cat_label,
                "total": len(cat_occs),
                "covered": len(cat_cov),
                "uncovered": len(cat_occs) - len(cat_cov),
                "pct": round(len(cat_cov) / len(cat_occs) * 100, 1),
            }

    return {
        "total": total,
        "covered": len(covered),
        "uncovered": total - len(covered),
        "percentage": round(pct, 2),
        "by_category": by_category,
        "false_positives_excluded": len(all_occs) - total,
    }

def format_report_md(report: dict, all_occs: list[Occurrence]) -> str:
valid = [o for o in all_occs if not o.false_positive]
ts = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
L = []

    L.append("# Registro de Medição — M2.1")
    L.append("**Percentual de operações críticas com tratamento de exceção**\n")
    L.append(f"- **Data:** {ts}")
    L.append(f"- **Repositório:** `2023-2-SuaGradeUnB`")
    L.append(f"- **Categorias analisadas:** Autenticação e Segurança | Regras de Negócio e BD | Infraestrutura e Integração | Web Scraping SIGAA")
    L.append(f"- **Escopo backend:** `api/` (excluídas: migrations, tests)")
    L.append(f"- **Escopo frontend:** `web/` (excluídas: imports/exports/comentários)")

    L.append("\n---\n")
    L.append("## 1. Resultado Consolidado\n")
    L.append("| Métrica | Valor |")
    L.append("|---------|-------|")
    L.append(f"| Total de operações críticas | {report['total']} |")
    L.append(f"| Com tratamento de exceção | {report['covered']} |")
    L.append(f"| Sem tratamento de exceção | {report['uncovered']} |")
    L.append(f"| **M2.1 (%)** | **{report['percentage']:.2f}%** |")
    L.append(f"| Falsos positivos excluídos | {report['false_positives_excluded']} |")

    L.append("\n---\n")
    L.append("## 2. Por Categoria\n")
    L.append("| Categoria | Total | Com exceção | Sem exceção | % coberta |")
    L.append("|-----------|------:|------------:|------------:|----------:|")
    for d in report["by_category"].values():
        L.append(f"| {d['label']} | {d['total']} | {d['covered']} | {d['uncovered']} | {d['pct']:.1f}% |")

    L.append("\n---\n")
    L.append("## 3. Detalhamento por Ocorrência\n")

    for cat_key, d in report["by_category"].items():
        L.append(f"### 3.{list(CATEGORIES.keys()).index(cat_key)+1} {d['label']}\n")
        cat_occs = [o for o in valid if o.category == cat_key]
        L.append("| # | Arquivo | Linha | Snippet | Exceção |")
        L.append("|--:|---------|------:|---------|:-------:|")
        for i, occ in enumerate(cat_occs, 1):
            snip = occ.code_snippet[:90].replace("|", "\\|")
            mark = "✓" if occ.has_exception_handling else "✗"
            L.append(f"| {i} | `{occ.file}` | {occ.line} | `{snip}` | {mark} |")
        L.append("")

    L.append("\n---\n")
    L.append("## 4. Operações SEM Tratamento de Exceção\n")
    uncov = [o for o in valid if not o.has_exception_handling]
    if uncov:
        L.append("| Arquivo | Linha | Categoria | Snippet |")
        L.append("|---------|------:|-----------|---------|")
        for occ in uncov:
            snip = occ.code_snippet[:100].replace("|", "\\|")
            L.append(f"| `{occ.file}` | {occ.line} | {CATEGORIES[occ.category]} | `{snip}` |")
    else:
        L.append("_Todas as operações críticas possuem tratamento de exceção._")

    L.append("\n---\n")
    L.append("## 5. Falsos Positivos Excluídos\n")
    fps = [o for o in all_occs if o.false_positive]
    if fps:
        L.append("| Arquivo | Linha | Motivo |")
        L.append("|---------|------:|--------|")
        for occ in fps:
            L.append(f"| `{occ.file}` | {occ.line} | {occ.note} |")
    else:
        L.append("_Nenhum falso positivo excluído._")

    L.append("\n---\n")
    L.append("## 6. Fórmula\n")
    L.append("```")
    L.append("M2.1 = (operações críticas com tratamento de exceção / total de operações críticas) × 100")
    L.append(f"     = ({report['covered']} / {report['total']}) × 100")
    L.append(f"     = {report['percentage']:.2f}%")
    L.append("```")

    return "\n".join(L)

# ---------------------------------------------------------------------------

# Main

# ---------------------------------------------------------------------------

def main():
print("=" _ 65)
print("M2.1 — Análise de operações críticas com tratamento de exceção")
print("=" _ 65)
print("Categorias:")
for k, v in CATEGORIES.items():
print(f" [{k}] {v}")

    all_occs: list[Occurrence] = []

    print("\n[1/2] Backend Python (AST)...")
    py_files = collect_py_files(BACKEND_ROOT)
    print(f"  {len(py_files)} arquivos .py encontrados")
    for pf in py_files:
        occs = analyze_python_file(pf)
        if occs:
            print(f"  {pf.relative_to(ROOT)}: {len(occs)}")
        all_occs.extend(occs)

    print("\n[2/2] Frontend TypeScript (grep)...")
    ts_files = collect_ts_files(FRONTEND_ROOT)
    print(f"  {len(ts_files)} arquivos .ts/.tsx encontrados")
    for tf in ts_files:
        occs = analyze_ts_file(tf)
        if occs:
            print(f"  {tf.relative_to(ROOT)}: {len(occs)}")
        all_occs.extend(occs)

    all_occs = mark_false_positives(all_occs)
    report = build_report(all_occs)

    print("\n" + "=" * 65)
    print("RESULTADO FINAL")
    print("=" * 65)
    print(f"  Total de operações críticas : {report['total']}")
    print(f"  Com tratamento de exceção   : {report['covered']}")
    print(f"  Sem tratamento de exceção   : {report['uncovered']}")
    print(f"  M2.1                        : {report['percentage']:.2f}%")
    print("-" * 65)
    for d in report["by_category"].values():
        print(f"  {d['label']:<40} {d['covered']:>3}/{d['total']:<3} = {d['pct']:>5.1f}%")
    print("=" * 65)

    out_md = ROOT / "scripts" / "m2_1_registro.md"
    out_json = ROOT / "scripts" / "m2_1_registro.json"

    out_md.write_text(format_report_md(report, all_occs), encoding="utf-8")
    print(f"\nRelatório Markdown : {out_md}")

    json_payload = {
        "metadata": {
            "metric": "M2.1",
            "generated_at": datetime.now().isoformat(),
            "categories": CATEGORIES,
        },
        "report": report,
        "occurrences": [
            {
                "file": o.file,
                "line": o.line,
                "category": o.category,
                "pattern_matched": o.pattern_matched,
                "code_snippet": o.code_snippet,
                "has_exception_handling": o.has_exception_handling,
                "false_positive": o.false_positive,
                "note": o.note,
            }
            for o in all_occs
        ],
    }
    out_json.write_text(json.dumps(json_payload, ensure_ascii=False, indent=2), encoding="utf-8")
    print(f"Dados brutos JSON  : {out_json}")

if **name** == "**main**":
main()
````

| Versão | Data       | Descrição         | Autor(es)       |
| :----: | :--------- | :---------------- | :-------------- |
|  1.0   | 2026-06-23 | Criação da pagina | Edilson Ribeiro |
