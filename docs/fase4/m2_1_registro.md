# Registro de Medição — M2.1
**Percentual de operações críticas com tratamento de exceção**

- **Data:** 2026-06-12 01:05:36
- **Repositório:** `2023-2-SuaGradeUnB`
- **Categorias analisadas:** Autenticação e Segurança | Regras de Negócio e BD | Infraestrutura e Integração | Web Scraping SIGAA
- **Escopo backend:** `api/` (excluídas: migrations, tests)
- **Escopo frontend:** `web/` (excluídas: imports/exports/comentários)

---

## 1. Resultado Consolidado

| Métrica | Valor |
|---------|-------|
| Total de operações críticas | 134 |
| Com tratamento de exceção | 24 |
| Sem tratamento de exceção | 110 |
| **M2.1 (%)** | **17.91%** |
| Falsos positivos excluídos | 9 |

---

## 2. Por Categoria

| Categoria | Total | Com exceção | Sem exceção | % coberta |
|-----------|------:|------------:|------------:|----------:|
| Autenticação e Segurança | 28 | 5 | 23 | 17.9% |
| Web Scraping SIGAA | 24 | 3 | 21 | 12.5% |
| Regras de Negócio e Banco de Dados | 44 | 7 | 37 | 15.9% |
| Infraestrutura e Integração | 38 | 9 | 29 | 23.7% |

---

## 3. Detalhamento por Ocorrência

### 3.1 Autenticação e Segurança

| # | Arquivo | Linha | Snippet | Exceção |
|--:|---------|------:|---------|:-------:|
| 1 | `api/api/management/commands/initadmin.py` | 9 | `username = config("ADMIN_NAME")` | ✗ |
| 2 | `api/api/management/commands/initadmin.py` | 10 | `if not len(User.objects.all().filter(username=username)):` | ✗ |
| 3 | `api/api/management/commands/initadmin.py` | 11 | `password = config("ADMIN_PASS")` | ✗ |
| 4 | `api/api/management/commands/initadmin.py` | 13 | `admin = User.objects.create_superuser(` | ✗ |
| 5 | `api/api/management/commands/initadmin.py` | 15 | `email=config("ADMIN_EMAIL")` | ✗ |
| 6 | `api/api/management/commands/initadmin.py` | 19 | `admin.save()` | ✗ |
| 7 | `api/api/views/delete_schedule.py` | 27 | `user = request.user` | ✗ |
| 8 | `api/api/views/get_schedules.py` | 28 | `user = request.user` | ✗ |
| 9 | `api/api/views/save_schedule.py` | 32 | `if not check_permission_to_save(request.user):` | ✗ |
| 10 | `api/api/views/save_schedule.py` | 59 | `user = request.user` | ✗ |
| 11 | `api/api/views/schedules.py` | 42 | `user = request.user` | ✗ |
| 12 | `api/core/settings/base.py` | 26 | `SECRET_KEY = config("DJANGO_SECRET_KEY")` | ✗ |
| 13 | `api/users/backends/google.py` | 24 | `if access_token != config('GOOGLE_OAUTH2_MOCK_TOKEN'):` | ✓ |
| 14 | `api/users/backends/google.py` | 25 | `response = requests.get(user_info_url, params=params)` | ✓ |
| 15 | `api/users/backends/google.py` | 36 | `user_data = response.json()` | ✓ |
| 16 | `api/users/backends/google.py` | 45 | `user, _ = User.objects.get_or_create(` | ✗ |
| 17 | `api/users/simplejwt/decorators.py` | 16 | `refresh_token = response.data.pop('refresh')` | ✗ |
| 18 | `api/users/simplejwt/decorators.py` | 17 | `response.set_cookie(key="refresh", value=refresh_token,` | ✗ |
| 19 | `api/users/views.py` | 67 | `backend = get_backend(kwargs['oauth2'])` | ✗ |
| 20 | `api/users/views.py` | 74 | `user_data = backend.get_user_data(token)` | ✗ |
| 21 | `api/users/views.py` | 76 | `user = backend.do_auth(user_data)` | ✗ |
| 22 | `api/users/views.py` | 79 | `refresh = serializer.get_token(user)` | ✗ |
| 23 | `api/users/views.py` | 101 | `request.data['refresh'] = request.COOKIES['refresh']` | ✓ |
| 24 | `api/users/views.py` | 114 | `serializer.is_valid(raise_exception=True)` | ✓ |
| 25 | `web/app/components/SignInSection.tsx` | 30 | `setAccessToken(retrieveAccessToken());` | ✗ |
| 26 | `web/app/components/SignInSection.tsx` | 35 | `registerWithGoogle(accessToken).then(response => {` | ✗ |
| 27 | `web/app/components/SignInSection.tsx` | 56 | `else signInWithGoogle(router);` | ✗ |
| 28 | `web/app/schedules/profile/page.tsx` | 75 | `<Button onClick={() => signInWithGoogle(router)} className='!shadow-none'>` | ✗ |

### 3.2 Web Scraping SIGAA

| # | Arquivo | Linha | Snippet | Exceção |
|--:|---------|------:|---------|:-------:|
| 1 | `api/api/management/commands/updatedb.py` | 70 | `departments_ids = get_list_of_departments()` | ✗ |
| 2 | `api/api/management/commands/updatedb.py` | 111 | `scraper = DisciplineWebScraper(department_id, year, period)` | ✗ |
| 3 | `api/api/management/commands/updatedb.py` | 125 | `disciplines_list = scraper.get_disciplines()` | ✗ |
| 4 | `api/utils/management/commands/updatemock.py` | 28 | `departments = wbp.get_list_of_departments()` | ✓ |
| 5 | `api/utils/management/commands/updatemock.py` | 32 | `discipline_scraper = wbp.DisciplineWebScraper(` | ✓ |
| 6 | `api/utils/management/commands/updatemock.py` | 34 | `response = discipline_scraper.get_response_from_disciplines_post_request()` | ✓ |
| 7 | `api/utils/management/commands/updatemock.py` | 54 | `return response.content.decode(encoding)` | ✗ |
| 8 | `api/utils/sessions.py` | 31 | `response = session.get(url=URL, headers=HEADERS) # Make a get request to the url` | ✗ |
| 9 | `api/utils/sessions.py` | 38 | `response = get_response(session) # Get the response from the request session` | ✗ |
| 10 | `api/utils/web_scraping.py` | 31 | `response = get_response(create_request_session())` | ✗ |
| 11 | `api/utils/web_scraping.py` | 33 | `soup = BeautifulSoup(response.content, "html.parser")` | ✗ |
| 12 | `api/utils/web_scraping.py` | 34 | `departments = soup.find("select", attrs={"id": "formTurma:inputDepto"})` | ✗ |
| 13 | `api/utils/web_scraping.py` | 60 | `self.session = create_request_session()` | ✗ |
| 14 | `api/utils/web_scraping.py` | 65 | `self.cookie = get_session_cookie(self.session)` | ✗ |
| 15 | `api/utils/web_scraping.py` | 74 | `initial_response = self.session.get(self.url, headers=HEADERS, cookies=self.cookie)` | ✗ |
| 16 | `api/utils/web_scraping.py` | 75 | `soup = BeautifulSoup(initial_response.content, "html.parser")` | ✗ |
| 17 | `api/utils/web_scraping.py` | 78 | `view_state_tag = soup.find("input", {"name": "javax.faces.ViewState"})` | ✗ |
| 18 | `api/utils/web_scraping.py` | 82 | `submit_button_tag = soup.find("input", {"value": "Buscar"})` | ✗ |
| 19 | `api/utils/web_scraping.py` | 97 | `self.response = self.session.post(` | ✗ |
| 20 | `api/utils/web_scraping.py` | 253 | `soup = BeautifulSoup(response.content, "html.parser")` | ✗ |
| 21 | `api/utils/web_scraping.py` | 255 | `tables = soup.find("table", attrs={"class": "listagem"})` | ✗ |
| 22 | `api/utils/web_scraping.py` | 264 | `self.get_response_from_disciplines_post_request()` | ✗ |
| 23 | `api/utils/web_scraping.py` | 292 | `self.get_response_from_disciplines_post_request()` | ✗ |
| 24 | `api/utils/web_scraping.py` | 293 | `self.make_web_scraping_of_disciplines(self.response)` | ✗ |

### 3.3 Regras de Negócio e Banco de Dados

| # | Arquivo | Linha | Snippet | Exceção |
|--:|---------|------:|---------|:-------:|
| 1 | `api/api/decorators.py` | 15 | `cache.delete(cache_key)` | ✓ |
| 2 | `api/api/decorators.py` | 19 | `queryset.delete()` | ✓ |
| 3 | `api/api/management/commands/updatedb.py` | 126 | `department = dbh.get_or_create_department(` | ✗ |
| 4 | `api/api/management/commands/updatedb.py` | 136 | `discipline = dbh.get_or_create_discipline(` | ✗ |
| 5 | `api/api/management/commands/updatedb.py` | 144 | `dbh.create_class(teachers=class_info["teachers"],` | ✗ |
| 6 | `api/api/models.py` | 17 | `cache.delete(kwargs['cache_key'])` | ✓ |
| 7 | `api/api/models.py` | 22 | `super(CustomModel, self).delete()` | ✓ |
| 8 | `api/api/models.py` | 48 | `super(Department, self).delete(*args, **kwargs)` | ✗ |
| 9 | `api/api/models.py` | 69 | `super(Discipline, self).save(*args, **kwargs)` | ✗ |
| 10 | `api/api/models.py` | 80 | `super(Discipline, self).delete(*args, **kwargs)` | ✗ |
| 11 | `api/api/models.py` | 120 | `super(Class, self).delete(*args, **kwargs)` | ✗ |
| 12 | `api/api/views/delete_schedule.py` | 29 | `return response.Response(status=status.HTTP_204_NO_CONTENT) if dbh.delete_schedule(user, i` | ✗ |
| 13 | `api/api/views/get_schedules.py` | 29 | `schedules = get_schedules(user)` | ✗ |
| 14 | `api/api/views/save_schedule.py` | 60 | `answer = save_schedule(user, valid_schedule)` | ✗ |
| 15 | `api/api/views/save_schedule.py` | 156 | `db_class = get_class_by_params(**key_args)` | ✗ |
| 16 | `api/api/views/save_schedule.py` | 220 | `schedules = dbh.get_schedules(user)` | ✗ |
| 17 | `api/api/views/schedules.py` | 43 | `schedules = dbh.get_schedules(user)` | ✗ |
| 18 | `api/api/views/views.py` | 53 | `disciplines = get_best_similarities_by_name(name, disciplines)` | ✗ |
| 19 | `api/api/views/views.py` | 56 | `disciplines = filter_disciplines_by_code(code=name[0])` | ✗ |
| 20 | `api/api/views/views.py` | 59 | `disciplines &= filter_disciplines_by_code(code=term)` | ✗ |
| 21 | `api/api/views/views.py` | 61 | `disciplines = filter_disciplines_by_code(name)` | ✗ |
| 22 | `api/api/views/views.py` | 69 | `disciplines = filter_disciplines_by_teacher(name)` | ✗ |
| 23 | `api/api/views/views.py` | 74 | `filtered_disciplines = filter_disciplines_by_year_and_period(` | ✗ |
| 24 | `api/users/backends/google.py` | 59 | `user.save()` | ✗ |
| 25 | `api/users/views.py` | 120 | `return response.Response(serializer.validated_data, status=status.HTTP_200_OK)` | ✗ |
| 26 | `api/utils/db_handler.py` | 20 | `return Department.objects.get_or_create(code=code, year=year, period=period)[0]` | ✗ |
| 27 | `api/utils/db_handler.py` | 25 | `return Discipline.objects.get_or_create(name=name, code=code, department=department)[0]` | ✗ |
| 28 | `api/utils/db_handler.py` | 31 | `return Class.objects.create(teachers=teachers, classroom=classroom, schedule=schedule,` | ✗ |
| 29 | `api/utils/db_handler.py` | 37 | `return Class.objects.filter(discipline=discipline)` | ✗ |
| 30 | `api/utils/db_handler.py` | 43 | `return Department.objects.filter(year=year, period=period)` | ✗ |
| 31 | `api/utils/db_handler.py` | 62 | `disciplines = Discipline.objects.all()` | ✗ |
| 32 | `api/utils/db_handler.py` | 111 | `Schedule.objects.get_or_create(user=user, classes=json_schedule)` | ✓ |
| 33 | `api/utils/db_handler.py` | 120 | `return Schedule.objects.filter(user=user).all()` | ✗ |
| 34 | `api/utils/db_handler.py` | 126 | `Schedule.objects.get(user=user, id=id).delete()` | ✓ |
| 35 | `api/utils/schedule_generator.py` | 65 | `_class = get_class_by_id(id=class_id)` | ✓ |
| 36 | `api/utils/search.py` | 20 | `values = self.model.objects.all()` | ✗ |
| 37 | `web/app/components/AsideSchedulePopUp/Form/InputForm.tsx` | 44 | `const data = await searchDiscipline(textSearch, year, period);` | ✗ |
| 38 | `web/app/components/SchedulePreview/SchedulePreview.tsx` | 135 | `getSchedules(user.access).then(response => {` | ✗ |
| 39 | `web/app/components/SchedulePreview/SchedulePreview.tsx` | 154 | `saveSchedule(props.schedules.localSchedule, user.access).then(response => {` | ✗ |
| 40 | `web/app/components/SchedulePreview/SchedulePreview.tsx` | 194 | `const response = await deleteSchedule(cloudSchedule?.id, user.access);` | ✗ |
| 41 | `web/app/components/SchedulePreview/SchedulePreview.tsx` | 197 | `getSchedules(user.access).then(response => {` | ✗ |
| 42 | `web/app/contexts/UserContext.tsx` | 63 | `getSchedules(user.access).then(response => {` | ✗ |
| 43 | `web/app/contexts/YearPeriodContext.tsx` | 21 | `retrieveYearAndPeriod().then(data => setPeriods(data));` | ✗ |
| 44 | `web/app/schedules/home/components/GenerateScheduleButton.tsx` | 55 | `generateSchedule(classes_id, [morningPreference, afternoonPreference, nightPreference]).th` | ✗ |

### 3.4 Infraestrutura e Integração

| # | Arquivo | Linha | Snippet | Exceção |
|--:|---------|------:|---------|:-------:|
| 1 | `api/api/management/commands/updatedb.py` | 116 | `cache_value = cache.get(cache_key)` | ✓ |
| 2 | `api/api/management/commands/updatedb.py` | 148 | `cache.set(cache_key, fingerprint, timeout=THIRTY_DAYS_IN_SECS)` | ✗ |
| 3 | `api/core/asgi.py` | 15 | `os.environ.setdefault('DJANGO_SETTINGS_MODULE', config('SETTINGS_FILE_PATH'))` | ✗ |
| 4 | `api/core/settings/base.py` | 63 | `"LOCATION": config("REDIS_CACHE_LOCATION"),` | ✗ |
| 5 | `api/core/settings/dev.py` | 25 | `"NAME": config("DB_NAME"),` | ✗ |
| 6 | `api/core/settings/dev.py` | 26 | `"USER": config("DB_USERNAME"),` | ✗ |
| 7 | `api/core/settings/dev.py` | 27 | `"PASSWORD": config("DB_PASSWORD"),` | ✗ |
| 8 | `api/core/settings/dev.py` | 28 | `"HOST": config("DB_HOSTNAME"),` | ✗ |
| 9 | `api/core/settings/dev.py` | 29 | `"PORT": config("DB_PORT", cast=int),` | ✗ |
| 10 | `api/core/settings/prod.py` | 43 | `"default": dj_database_url.config(` | ✗ |
| 11 | `api/core/wsgi.py` | 15 | `os.environ.setdefault('DJANGO_SETTINGS_MODULE', config('SETTINGS_FILE_PATH'))` | ✗ |
| 12 | `api/utils/management/commands/updatemock.py` | 21 | `os.remove(current_path / f"mock/sigaa.html")` | ✓ |
| 13 | `api/utils/management/commands/updatemock.py` | 22 | `os.remove(current_path / "mock/infos.json")` | ✓ |
| 14 | `api/utils/management/commands/updatemock.py` | 31 | `with open(current_path / f"mock/sigaa.html", "a") as mock_file:` | ✓ |
| 15 | `api/utils/management/commands/updatemock.py` | 38 | `mock_file.write(striped_response)` | ✓ |
| 16 | `api/utils/management/commands/updatemock.py` | 40 | `with open(current_path / "mock/infos.json", "a") as info_file:` | ✓ |
| 17 | `api/utils/management/commands/updatemock.py` | 46 | `info_file.write(json.dumps(data))` | ✓ |
| 18 | `api/utils/sessions.py` | 22 | `session = Session() # Create a persistent request session` | ✗ |
| 19 | `api/utils/sessions.py` | 23 | `retry = Retry(connect=3, backoff_factor=0.5) # Create a retry object` | ✗ |
| 20 | `api/utils/sessions.py` | 24 | `adapter = HTTPAdapter(max_retries=retry) # Try to make the request 3 times` | ✗ |
| 21 | `api/utils/sessions.py` | 25 | `session.mount('https://', adapter) # Mount the adapter to the session` | ✗ |
| 22 | `api/utils/views.py` | 14 | `with open(mocked_html_path / f"mock/{actual_path[path]}.html", 'r') as html_file:` | ✗ |
| 23 | `api/utils/views.py` | 15 | `content = html_file.read()` | ✗ |
| 24 | `web/app/contexts/SchedulesContext.tsx` | 47 | `const localJSON = JSON.parse(localStorage.getItem('schedules') \|\| '[]');` | ✗ |
| 25 | `web/app/contexts/SchedulesContext.tsx` | 62 | `localStorage.setItem('schedules', JSON.stringify(uniqueSchedules));` | ✗ |
| 26 | `web/app/contexts/SchedulesContext.tsx` | 63 | `} else localStorage.setItem('schedules', JSON.stringify(newSchedules));` | ✗ |
| 27 | `web/app/contexts/SchedulesContext.tsx` | 65 | `const localJSON = JSON.parse(localStorage.getItem('schedules') \|\| '[]');` | ✗ |
| 28 | `web/app/contexts/SchedulesContext.tsx` | 74 | `data[index].classes = JSON.parse(schedule.classes);` | ✗ |
| 29 | `web/app/contexts/UserContext.tsx` | 45 | `request.post('/users/login/', {}, settings).then(response => {` | ✗ |
| 30 | `web/app/utils/api/deleteSchedule.ts` | 5 | `const response = await request.delete(` | ✗ |
| 31 | `web/app/utils/api/generateSchedule.ts` | 17 | `const response = await request.post('/courses/schedules/generate/', body, settings);` | ✗ |
| 32 | `web/app/utils/api/getSchedules.ts` | 5 | `const response = request.get('/courses/schedules/', settingsWithAuth(access_token));` | ✗ |
| 33 | `web/app/utils/api/logout.ts` | 22 | `request.post('/users/logout/', {}, settings).then(response => {` | ✗ |
| 34 | `web/app/utils/api/registerWithGoogle.ts` | 10 | `const response = await request.post('/users/register/google/', body, settings);` | ✗ |
| 35 | `web/app/utils/api/retrieveYearAndPeriod.ts` | 12 | `const response = await request.get('/courses/year-period/', settings);` | ✓ |
| 36 | `web/app/utils/api/saveSchedule.ts` | 7 | `const response = await request.post('/courses/schedules/', schedule, settingsWithAuth(acce` | ✗ |
| 37 | `web/app/utils/api/searchDiscipline.ts` | 32 | `const response = await request.get('/courses/', {` | ✓ |
| 38 | `web/app/utils/request.ts` | 3 | `const request = axios.create({` | ✗ |


---

## 4. Operações SEM Tratamento de Exceção

| Arquivo | Linha | Categoria | Snippet |
|---------|------:|-----------|---------|
| `api/api/management/commands/initadmin.py` | 9 | Autenticação e Segurança | `username = config("ADMIN_NAME")` |
| `api/api/management/commands/initadmin.py` | 10 | Autenticação e Segurança | `if not len(User.objects.all().filter(username=username)):` |
| `api/api/management/commands/initadmin.py` | 11 | Autenticação e Segurança | `password = config("ADMIN_PASS")` |
| `api/api/management/commands/initadmin.py` | 13 | Autenticação e Segurança | `admin = User.objects.create_superuser(` |
| `api/api/management/commands/initadmin.py` | 15 | Autenticação e Segurança | `email=config("ADMIN_EMAIL")` |
| `api/api/management/commands/initadmin.py` | 19 | Autenticação e Segurança | `admin.save()` |
| `api/api/management/commands/updatedb.py` | 70 | Web Scraping SIGAA | `departments_ids = get_list_of_departments()` |
| `api/api/management/commands/updatedb.py` | 111 | Web Scraping SIGAA | `scraper = DisciplineWebScraper(department_id, year, period)` |
| `api/api/management/commands/updatedb.py` | 125 | Web Scraping SIGAA | `disciplines_list = scraper.get_disciplines()` |
| `api/api/management/commands/updatedb.py` | 126 | Regras de Negócio e Banco de Dados | `department = dbh.get_or_create_department(` |
| `api/api/management/commands/updatedb.py` | 136 | Regras de Negócio e Banco de Dados | `discipline = dbh.get_or_create_discipline(` |
| `api/api/management/commands/updatedb.py` | 144 | Regras de Negócio e Banco de Dados | `dbh.create_class(teachers=class_info["teachers"],` |
| `api/api/management/commands/updatedb.py` | 148 | Infraestrutura e Integração | `cache.set(cache_key, fingerprint, timeout=THIRTY_DAYS_IN_SECS)` |
| `api/api/models.py` | 48 | Regras de Negócio e Banco de Dados | `super(Department, self).delete(*args, **kwargs)` |
| `api/api/models.py` | 69 | Regras de Negócio e Banco de Dados | `super(Discipline, self).save(*args, **kwargs)` |
| `api/api/models.py` | 80 | Regras de Negócio e Banco de Dados | `super(Discipline, self).delete(*args, **kwargs)` |
| `api/api/models.py` | 120 | Regras de Negócio e Banco de Dados | `super(Class, self).delete(*args, **kwargs)` |
| `api/api/views/delete_schedule.py` | 27 | Autenticação e Segurança | `user = request.user` |
| `api/api/views/delete_schedule.py` | 29 | Regras de Negócio e Banco de Dados | `return response.Response(status=status.HTTP_204_NO_CONTENT) if dbh.delete_schedule(user, id) else re` |
| `api/api/views/get_schedules.py` | 28 | Autenticação e Segurança | `user = request.user` |
| `api/api/views/get_schedules.py` | 29 | Regras de Negócio e Banco de Dados | `schedules = get_schedules(user)` |
| `api/api/views/save_schedule.py` | 32 | Autenticação e Segurança | `if not check_permission_to_save(request.user):` |
| `api/api/views/save_schedule.py` | 59 | Autenticação e Segurança | `user = request.user` |
| `api/api/views/save_schedule.py` | 60 | Regras de Negócio e Banco de Dados | `answer = save_schedule(user, valid_schedule)` |
| `api/api/views/save_schedule.py` | 156 | Regras de Negócio e Banco de Dados | `db_class = get_class_by_params(**key_args)` |
| `api/api/views/save_schedule.py` | 220 | Regras de Negócio e Banco de Dados | `schedules = dbh.get_schedules(user)` |
| `api/api/views/schedules.py` | 42 | Autenticação e Segurança | `user = request.user` |
| `api/api/views/schedules.py` | 43 | Regras de Negócio e Banco de Dados | `schedules = dbh.get_schedules(user)` |
| `api/api/views/views.py` | 53 | Regras de Negócio e Banco de Dados | `disciplines = get_best_similarities_by_name(name, disciplines)` |
| `api/api/views/views.py` | 56 | Regras de Negócio e Banco de Dados | `disciplines = filter_disciplines_by_code(code=name[0])` |
| `api/api/views/views.py` | 59 | Regras de Negócio e Banco de Dados | `disciplines &= filter_disciplines_by_code(code=term)` |
| `api/api/views/views.py` | 61 | Regras de Negócio e Banco de Dados | `disciplines = filter_disciplines_by_code(name)` |
| `api/api/views/views.py` | 69 | Regras de Negócio e Banco de Dados | `disciplines = filter_disciplines_by_teacher(name)` |
| `api/api/views/views.py` | 74 | Regras de Negócio e Banco de Dados | `filtered_disciplines = filter_disciplines_by_year_and_period(` |
| `api/core/asgi.py` | 15 | Infraestrutura e Integração | `os.environ.setdefault('DJANGO_SETTINGS_MODULE', config('SETTINGS_FILE_PATH'))` |
| `api/core/settings/base.py` | 26 | Autenticação e Segurança | `SECRET_KEY = config("DJANGO_SECRET_KEY")` |
| `api/core/settings/base.py` | 63 | Infraestrutura e Integração | `"LOCATION": config("REDIS_CACHE_LOCATION"),` |
| `api/core/settings/dev.py` | 25 | Infraestrutura e Integração | `"NAME": config("DB_NAME"),` |
| `api/core/settings/dev.py` | 26 | Infraestrutura e Integração | `"USER": config("DB_USERNAME"),` |
| `api/core/settings/dev.py` | 27 | Infraestrutura e Integração | `"PASSWORD": config("DB_PASSWORD"),` |
| `api/core/settings/dev.py` | 28 | Infraestrutura e Integração | `"HOST": config("DB_HOSTNAME"),` |
| `api/core/settings/dev.py` | 29 | Infraestrutura e Integração | `"PORT": config("DB_PORT", cast=int),` |
| `api/core/settings/prod.py` | 43 | Infraestrutura e Integração | `"default": dj_database_url.config(` |
| `api/core/wsgi.py` | 15 | Infraestrutura e Integração | `os.environ.setdefault('DJANGO_SETTINGS_MODULE', config('SETTINGS_FILE_PATH'))` |
| `api/users/backends/google.py` | 45 | Autenticação e Segurança | `user, _ = User.objects.get_or_create(` |
| `api/users/backends/google.py` | 59 | Regras de Negócio e Banco de Dados | `user.save()` |
| `api/users/simplejwt/decorators.py` | 16 | Autenticação e Segurança | `refresh_token = response.data.pop('refresh')` |
| `api/users/simplejwt/decorators.py` | 17 | Autenticação e Segurança | `response.set_cookie(key="refresh", value=refresh_token,` |
| `api/users/views.py` | 67 | Autenticação e Segurança | `backend = get_backend(kwargs['oauth2'])` |
| `api/users/views.py` | 74 | Autenticação e Segurança | `user_data = backend.get_user_data(token)` |
| `api/users/views.py` | 76 | Autenticação e Segurança | `user = backend.do_auth(user_data)` |
| `api/users/views.py` | 79 | Autenticação e Segurança | `refresh = serializer.get_token(user)` |
| `api/users/views.py` | 120 | Regras de Negócio e Banco de Dados | `return response.Response(serializer.validated_data, status=status.HTTP_200_OK)` |
| `api/utils/db_handler.py` | 20 | Regras de Negócio e Banco de Dados | `return Department.objects.get_or_create(code=code, year=year, period=period)[0]` |
| `api/utils/db_handler.py` | 25 | Regras de Negócio e Banco de Dados | `return Discipline.objects.get_or_create(name=name, code=code, department=department)[0]` |
| `api/utils/db_handler.py` | 31 | Regras de Negócio e Banco de Dados | `return Class.objects.create(teachers=teachers, classroom=classroom, schedule=schedule,` |
| `api/utils/db_handler.py` | 37 | Regras de Negócio e Banco de Dados | `return Class.objects.filter(discipline=discipline)` |
| `api/utils/db_handler.py` | 43 | Regras de Negócio e Banco de Dados | `return Department.objects.filter(year=year, period=period)` |
| `api/utils/db_handler.py` | 62 | Regras de Negócio e Banco de Dados | `disciplines = Discipline.objects.all()` |
| `api/utils/db_handler.py` | 120 | Regras de Negócio e Banco de Dados | `return Schedule.objects.filter(user=user).all()` |
| `api/utils/management/commands/updatemock.py` | 54 | Web Scraping SIGAA | `return response.content.decode(encoding)` |
| `api/utils/search.py` | 20 | Regras de Negócio e Banco de Dados | `values = self.model.objects.all()` |
| `api/utils/sessions.py` | 22 | Infraestrutura e Integração | `session = Session() # Create a persistent request session` |
| `api/utils/sessions.py` | 23 | Infraestrutura e Integração | `retry = Retry(connect=3, backoff_factor=0.5) # Create a retry object` |
| `api/utils/sessions.py` | 24 | Infraestrutura e Integração | `adapter = HTTPAdapter(max_retries=retry) # Try to make the request 3 times` |
| `api/utils/sessions.py` | 25 | Infraestrutura e Integração | `session.mount('https://', adapter) # Mount the adapter to the session` |
| `api/utils/sessions.py` | 31 | Web Scraping SIGAA | `response = session.get(url=URL, headers=HEADERS) # Make a get request to the url` |
| `api/utils/sessions.py` | 38 | Web Scraping SIGAA | `response = get_response(session) # Get the response from the request session` |
| `api/utils/views.py` | 14 | Infraestrutura e Integração | `with open(mocked_html_path / f"mock/{actual_path[path]}.html", 'r') as html_file:` |
| `api/utils/views.py` | 15 | Infraestrutura e Integração | `content = html_file.read()` |
| `api/utils/web_scraping.py` | 31 | Web Scraping SIGAA | `response = get_response(create_request_session())` |
| `api/utils/web_scraping.py` | 33 | Web Scraping SIGAA | `soup = BeautifulSoup(response.content, "html.parser")` |
| `api/utils/web_scraping.py` | 34 | Web Scraping SIGAA | `departments = soup.find("select", attrs={"id": "formTurma:inputDepto"})` |
| `api/utils/web_scraping.py` | 60 | Web Scraping SIGAA | `self.session = create_request_session()` |
| `api/utils/web_scraping.py` | 65 | Web Scraping SIGAA | `self.cookie = get_session_cookie(self.session)` |
| `api/utils/web_scraping.py` | 74 | Web Scraping SIGAA | `initial_response = self.session.get(self.url, headers=HEADERS, cookies=self.cookie)` |
| `api/utils/web_scraping.py` | 75 | Web Scraping SIGAA | `soup = BeautifulSoup(initial_response.content, "html.parser")` |
| `api/utils/web_scraping.py` | 78 | Web Scraping SIGAA | `view_state_tag = soup.find("input", {"name": "javax.faces.ViewState"})` |
| `api/utils/web_scraping.py` | 82 | Web Scraping SIGAA | `submit_button_tag = soup.find("input", {"value": "Buscar"})` |
| `api/utils/web_scraping.py` | 97 | Web Scraping SIGAA | `self.response = self.session.post(` |
| `api/utils/web_scraping.py` | 253 | Web Scraping SIGAA | `soup = BeautifulSoup(response.content, "html.parser")` |
| `api/utils/web_scraping.py` | 255 | Web Scraping SIGAA | `tables = soup.find("table", attrs={"class": "listagem"})` |
| `api/utils/web_scraping.py` | 264 | Web Scraping SIGAA | `self.get_response_from_disciplines_post_request()` |
| `api/utils/web_scraping.py` | 292 | Web Scraping SIGAA | `self.get_response_from_disciplines_post_request()` |
| `api/utils/web_scraping.py` | 293 | Web Scraping SIGAA | `self.make_web_scraping_of_disciplines(self.response)` |
| `web/app/components/AsideSchedulePopUp/Form/InputForm.tsx` | 44 | Regras de Negócio e Banco de Dados | `const data = await searchDiscipline(textSearch, year, period);` |
| `web/app/components/SchedulePreview/SchedulePreview.tsx` | 135 | Regras de Negócio e Banco de Dados | `getSchedules(user.access).then(response => {` |
| `web/app/components/SchedulePreview/SchedulePreview.tsx` | 154 | Regras de Negócio e Banco de Dados | `saveSchedule(props.schedules.localSchedule, user.access).then(response => {` |
| `web/app/components/SchedulePreview/SchedulePreview.tsx` | 194 | Regras de Negócio e Banco de Dados | `const response = await deleteSchedule(cloudSchedule?.id, user.access);` |
| `web/app/components/SchedulePreview/SchedulePreview.tsx` | 197 | Regras de Negócio e Banco de Dados | `getSchedules(user.access).then(response => {` |
| `web/app/components/SignInSection.tsx` | 30 | Autenticação e Segurança | `setAccessToken(retrieveAccessToken());` |
| `web/app/components/SignInSection.tsx` | 35 | Autenticação e Segurança | `registerWithGoogle(accessToken).then(response => {` |
| `web/app/components/SignInSection.tsx` | 56 | Autenticação e Segurança | `else signInWithGoogle(router);` |
| `web/app/contexts/SchedulesContext.tsx` | 47 | Infraestrutura e Integração | `const localJSON = JSON.parse(localStorage.getItem('schedules') \|\| '[]');` |
| `web/app/contexts/SchedulesContext.tsx` | 62 | Infraestrutura e Integração | `localStorage.setItem('schedules', JSON.stringify(uniqueSchedules));` |
| `web/app/contexts/SchedulesContext.tsx` | 63 | Infraestrutura e Integração | `} else localStorage.setItem('schedules', JSON.stringify(newSchedules));` |
| `web/app/contexts/SchedulesContext.tsx` | 65 | Infraestrutura e Integração | `const localJSON = JSON.parse(localStorage.getItem('schedules') \|\| '[]');` |
| `web/app/contexts/SchedulesContext.tsx` | 74 | Infraestrutura e Integração | `data[index].classes = JSON.parse(schedule.classes);` |
| `web/app/contexts/UserContext.tsx` | 45 | Infraestrutura e Integração | `request.post('/users/login/', {}, settings).then(response => {` |
| `web/app/contexts/UserContext.tsx` | 63 | Regras de Negócio e Banco de Dados | `getSchedules(user.access).then(response => {` |
| `web/app/contexts/YearPeriodContext.tsx` | 21 | Regras de Negócio e Banco de Dados | `retrieveYearAndPeriod().then(data => setPeriods(data));` |
| `web/app/schedules/home/components/GenerateScheduleButton.tsx` | 55 | Regras de Negócio e Banco de Dados | `generateSchedule(classes_id, [morningPreference, afternoonPreference, nightPreference]).then((respon` |
| `web/app/schedules/profile/page.tsx` | 75 | Autenticação e Segurança | `<Button onClick={() => signInWithGoogle(router)} className='!shadow-none'>` |
| `web/app/utils/api/deleteSchedule.ts` | 5 | Infraestrutura e Integração | `const response = await request.delete(` |
| `web/app/utils/api/generateSchedule.ts` | 17 | Infraestrutura e Integração | `const response = await request.post('/courses/schedules/generate/', body, settings);` |
| `web/app/utils/api/getSchedules.ts` | 5 | Infraestrutura e Integração | `const response = request.get('/courses/schedules/', settingsWithAuth(access_token));` |
| `web/app/utils/api/logout.ts` | 22 | Infraestrutura e Integração | `request.post('/users/logout/', {}, settings).then(response => {` |
| `web/app/utils/api/registerWithGoogle.ts` | 10 | Infraestrutura e Integração | `const response = await request.post('/users/register/google/', body, settings);` |
| `web/app/utils/api/saveSchedule.ts` | 7 | Infraestrutura e Integração | `const response = await request.post('/courses/schedules/', schedule, settingsWithAuth(access_token))` |
| `web/app/utils/request.ts` | 3 | Infraestrutura e Integração | `const request = axios.create({` |

---

## 5. Falsos Positivos Excluídos

| Arquivo | Linha | Motivo |
|---------|------:|--------|
| `web/app/utils/api/deleteSchedule.ts` | 4 | import/export/comment |
| `web/app/utils/api/generateSchedule.ts` | 8 | import/export/comment |
| `web/app/utils/api/getSchedules.ts` | 4 | import/export/comment |
| `web/app/utils/api/registerWithGoogle.ts` | 5 | import/export/comment |
| `web/app/utils/api/retrieveYearAndPeriod.ts` | 10 | import/export/comment |
| `web/app/utils/api/saveSchedule.ts` | 6 | import/export/comment |
| `web/app/utils/api/searchDiscipline.ts` | 24 | import/export/comment |
| `web/app/utils/retrieveAccessToken.ts` | 1 | import/export/comment |
| `web/app/utils/signInWithGoogle.ts` | 3 | import/export/comment |