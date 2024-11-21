# Vue приложение с формой, в которой реализована валидация с использованием Vuelidate
=====================

Шаги для запуска приложения
```
1) Перейти в каталог с проектом
2) Установить node: npm install
3) Запустить веб сервер: npm run serve
4) В браузере открыть тот localhost который предоставит сервер (например http://localhost:8181)
```

Описание файлов проекта
```
1) src/shims-vue.d.ts - декларация типов для Vue-компонентов в TypeScript
2) src/App.vue - собой основной компонент Vue-приложения
3) src/main.ts - точка входа для Vue-приложенияe, где создается экземпляр Vue-приложения.
4) src/router/index.ts - отвечает за настройку маршрутизации во Vue-приложении
5) src/interfaces/FormField.interface.ts - определяет TypeScript интерфейс для формы, что помогает типизировать данные поля формы
6) src/components/Form.vue - это компонент формы, который рендерит поля формы на основе входного свойства formFields, обрабатывает ввод данных от пользователей и осуществляет валидацию
7) src/views/Home.vue - представление, в котором отображается форма, используя компонент формы
```
