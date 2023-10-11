# ToDo List

[Демо] (https://pagesid.github.io/Todolist/)

## О проекте
Список задача (ToDoList). Прекрасный и минималистичный ToDo, который функционирует как необходимо. Можем видеть заверешенные и удаленные задачи, при желании которые можно восстанавливать. Не создаются "пустые" задачи.

Данный проект является моим любимчиком, так как сам пользуюсь Todo List и потратил много времени для поиска более пододящего задачника.

## HTML
Структура ToDo List очень проста.
```
<div class="todo">
  <div class="todo__input">
    <input class="todo__text" type="text">
    <span class="todo__add"></span>
  </div>
  <select class="todo__options">
    <option value="active">активные</option>
    <option value="completed">завершённые</option>
    <option value="deleted">удалённые</option>
  </select>
  ...
</div>
```
Данные классы предназначеные для управления списком и созданием задач.
```
<ul class="todo__items" data-todo-option="active"></ul>
```
В .todo__items будут добавляться задачи.

HTML-код самой задачи:
```
`<li class="todo__item" data-todo-state="active">
		  <span class="todo__task">
			 ${text}
			 <span class="todo__date" data-todo-date="${date}">
				<span>добавлено: ${new Date().toLocaleString().slice(0, -3)}</span>
			 </span>
		  </span>
		  <span class="todo__action todo__action_restore" data-todo-action="active"></span>
		  <span class="todo__action todo__action_complete" data-todo-action="completed"></span>
		  <span class="todo__action todo__action_delete" data-todo-action="deleted"></span></li>`;
```
Значение атрибута data-todo-state будет определять состояние задачи:

1. active – активная;
2. completed – выполненная;
3. deleted – удалённая.

.todo__task – это элемент, который содержит текст задачи, а .todo__action – это кнопки для выполнения действий над задачей.

С помощью них мы можем восстановить задачу (перевести её в активное состояние), отметить задачу как завершённую и удалить. Действие, которое выполняет кнопка .todo__action определяется значением атрибута data-todo-action.

## CSS
С помощью следующих стилей регулируется видимость списков.
 Переключение отображение задач в .todo__items:
```
[data-todo-state=active] .todo__action_restore,
[data-todo-state=completed] .todo__action_complete,
[data-todo-state=deleted] .todo__action_complete {
  display: none;
}
```

 Скрытие кнопок для задач, которые не должны показываться для определённых состояний, осуществляется следующим образом:
```
[data-todo-option=active] .todo__item:not([data-todo-state=active]),
[data-todo-option=completed] .todo__item:not([data-todo-state=completed]),
[data-todo-option=deleted] .todo__item:not([data-todo-state=deleted]) {
  display: none;
}
```

## JavaScript
В данном проекте была использован чистый JavaScript.

#### _Порядок выполнения и функции кода в проекте_:

Создаем объект todo и помещам туда следубщие методы:
```
const todo = {
  action(e) {},
  add() {},
  create(text) {},
  init() {},
  update() {},
  save() {}
};
```
Начнём с init(). Данный метод будет осуществлять инициализацию Todo List.

Он выполняет следующие вещи:

1. получает из localStorage сохранённый список дел (вставляет его в .todo__items);
2. назначает обработчик события change на элемент .todo__options; в качестве обработчика используется update;
3. назначает обработчик события click на document; в качестве обработчика выступает action.

Метод create() будет возвращать HTML код самой задачи с указанным текстом.

save() получает содержимое .todo__items и устанавливает его в localStorage.

update() используется в качестве обработчика.

add() добавляет при нажатии на кнопку задачу в список .todo__items. Для создания HTML-кода задачи используется create().

action вызывается, когда происходит событие click на документе.

Параметр e – это объект события event. Его создаёт браузер и передаёт его в качестве первого аргумента action.

В коде e.target – это элемент, по которому кликнули. Так как нам нужны не любые клики, а только по определённым элементам, то используем следующие условия

Если пользователь кликнул по .todo__add, то выполним следующие действия:
```
this.add();
this.save();
```

add() добавляет в список .todo__items новую задачу, а save() сохраняет все задачи (содержимое .todo__items) в localStorage.

Когда пользователь кликнул на .todo__action выполняется следующий код:
```
const action = target.dataset.todoAction;
const elemItem = target.closest('.todo__item');
if (action === 'deleted' && elemItem.dataset.todoState === 'deleted') {
  elemItem.remove();
} else {
  elemItem.dataset.todoState = action;
}
this.save();
```

Первое – получаем действие, которое нужно выполнить. Оно у нас находится в атрибуте data-todo-action. Далее находим элемент .todo__item и сохраняем его в переменную elemItem. Этот элемент нам понадобится дальше. После этого, если действие delete и состояние задачи delete, то удаляем элемент. В противном случае установим атрибуту data-todo-state элемента .todo__item значение action. В конце сохраним все изменения в localStorage с помощью save().

Последнее что нужно сделать чтобы Todo работал это вызвать init:

```
todo.init();
```
