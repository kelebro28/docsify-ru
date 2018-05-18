# Написание плагина

Плагин - это просто функция, которая воспринимает `hook` как аргумент. Hook поддерживает обработку асинхронных задач.

##  Полная конфигурация

```js
window.$docsify = {
 plugins: [
  function (hook, vm) {
    hook.init(function() {
      // Вызывается при запуске скрипта, запускается только один раз, без аргументов
    })

    hook.beforeEach(function(content) {
      // Вызывается каждый раз перед разбором файла Markdown.
      // ...
      return content
    })

    hook.afterEach(function(html, next) {
      // Вызывается каждый раз после разбора файла Markdown.
      // beforeEach и afterEach поддерживают асинхронный вызов.
      // ...
      // Вызывается `next(html)` когда задача выполнена.
      next(html)
    })

    hook.doneEach(function() {
      // Вызывается каждый раз после полной загрузки данных, никаких аргументов,
      // ...
    })

    hook.mounted(function() {
      // Вызывается после первоначального завершения. Только триггер на один раз, никаких аргументов.
    })

    hook.ready(function() {
      // Вызывается после первоначального завершения, никаких аргументов.
    })
  }
 ]
}
```

!> Вы можете получить внутренние методы через `window.Docsify`. Получить текущий экземпляр через второй аргумент.

## Пример

#### подвал

Добавляет компонент нижнего колонтитула на каждую страницу.

```js
window.$docsify = {
  plugins: [
    function (hook) {
      var footer = [
        '<hr/>',
        '<footer>',
        '<span><a href="https://github.com/QingWei-Li">cinwell</a> &copy;2017.</span>',
        '<span>Proudly published with <a href="https://github.com/QingWei-Li/docsify" target="_blank">docsify</a>.</span>',
        '</footer>'
      ].join('')

      hook.afterEach(function (html) {
        return html + footer
      })
    }
  ]
}
```

### Кнопка редактирования

```js
window.$docsify = {
  plugins: [
    function(hook, vm) {
      hook.beforeEach(function (html) {
        var url = 'https://github.com/QingWei-Li/docsify/blob/master/docs' + vm.route.file
        var editHtml = '[📝 РЕДАКТИРОВАТЬ ДОКУМЕНТ](' + url + ')\n'

        return editHtml
          + html
          + '\n----\n'
          + 'Последняя модификация {docsify-updated} '
          + editHtml
      })
    }
  ]
}
```


