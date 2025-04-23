---
title: <%*
/*
 * © 2025 Jarwix
 *
 * Этот шаблон предоставляется по лицензии MIT.
 * Вы можете использовать, изменять и распространять его свободно,
 * при условии обязательного указания автора и включения этой лицензии.
 *
 * Author: Jarwix (https://t.me/sdvghack)
 */
  const surname = await tp.system.prompt("Введите фамилию");
  const name = await tp.system.prompt("Введите имя");
  const title = `${surname.trim()} ${name.trim()}`;
  tR += title;
%>
created: <% tp.date.now("YYYY-MM-DD HH:mm") %>
<%*
let categories = [
    "Друзья", 
    "Коллеги", 
    "Семья", 
    "Клиенты", 
    "Партнёры", 
    "Знакомые", 
    "Образование", 
    "Другое"
];

let selectedCategory = await tp.system.suggester(
    categories, 
    categories, 
    false, 
    "Выберите категорию контакта"
);

let rolesByCategory = {
    "Друзья": ["Друг", "Близкий друг", "Романтический партнёр"],
    "Коллеги": ["Коллега", "Руководитель", "Подчинённый", "Наставник", "Ученик"],
    "Семья": ["Родственник", "Супруг/Супруга", "Родитель", "Ребёнок", "Брат/Сестра"],
    "Клиенты": ["Клиент", "Заказчик", "Контрагент"],
    "Партнёры": ["Деловой партнёр", "Инвестор", "Соавтор"],
    "Знакомые": ["Знакомый", "Сосед", "Бывший коллега"],
    "Образование": ["Учитель", "Преподаватель", "Наставник", "Тренер", "Репетитор"],
    "Другое": ["Коуч", "Советник", "Вдохновитель", "Спикер", "Онлайн-знакомый", "Знаменитость", "Другое"]
};

let roles = rolesByCategory[selectedCategory] || ["Другое"];
let selectedRole = await tp.system.suggester(
    roles, 
    roles, 
    false, 
    "Выберите роль контакта"
);

tR += `category: ${selectedCategory}\nrole: ${selectedRole}`;
%>

birthday: <%*
  let birthday = await tp.system.prompt("Введите день рождения (например, 1990-05-15)", "-");
  birthday = birthday.trim();
  if (!birthday.match(/^\d{4}-\d{2}-\d{2}$/)) {
    birthday = "";
  }
  tR += birthday;
%>
tags:
  - Rolodex
  - Person
  - Контакт
---
# Карточка: <% title %>

**Дата создания:** <% tp.date.now("YYYY-MM-DD") %>

---
## Основная информация

**Место работы/учебы:** <%*
let workplace = await tp.system.prompt("Введите место работы/учебы (например, Поликлиника в Химках)", "-");
tR += workplace.trim();
%>
**Должность:** <%*
let jobtitle = await tp.system.prompt("Введите название должности (например, врач)", "-");
tR += jobtitle.trim();
%>
**Знакомство:** <%*
let meeting = await tp.system.prompt("Как именно познакомились (в чате, лично, можно краткую историю)", "-");
tR += meeting.trim();
%>
**Дата знакомства:** <%*
let timemeeting = await tp.system.prompt("Введите дату знакомства (год, если не получается вспомнить точную дату)", "-");
tR += timemeeting.trim();
%>

---
## Контактные данные

**Телефон:** <%* 
let phone = await tp.system.prompt("Введите номер телефона", "Не указан");
tR += phone.trim();
%>
**Email:** <%* 
let email = await tp.system.prompt("Введите email", "Не указан");
tR += email.trim();
%>
**Социальные сети:** <%* 
let socials = await tp.system.prompt("Введите ссылки на соцсети (или оставьте пустым)", "Не указаны");
tR += socials.trim();
%>

---
## Заметки

<%*
let notes = [];
let continueAdding = true;

while (continueAdding) {
    const note = await tp.system.prompt("Введите заметку о человеке, его хобби, отношение к Вам. Можно вводить несколько пунктов через Enter. Для завершения оставьте пустое поле.");
    
    if (note) {
        notes.push(note);
    } else {
        continueAdding = false;
    }
}

if (notes.length > 0) {
    tR += notes.map(n => `- ${n}`).join("\n");
} else {
    tR += "- Нет заметок";
}
%>

---
## Встречи и события

<%* 
let flag = true;
let events = await tp.system.prompt("Добавить событие или встречу? (или оставьте пустым)");
if (events) {
    tR += `- [ ] ${events} #rolodex\n`;
    flag = false;
}
if (!birthday) {
    tR += `- [ ] Узнать и добавить день рождения #rolodex\n`;
    flag = false;
}
if (notes.length == 0) {
    tR += `- [ ] Нет заметок — необходимо узнать о человеке побольше #rolodex\n`;
    flag = false;
}
if (flag){
    tR += `- [ ] Нужно что-то запланировать совместное с человеком? #rolodex\n`;
}
%>
<%*
  let fileName = `${title}`;
  let startFolder = `Картотека`;
  const firstLetter = fileName.charAt(0).toUpperCase();

let targetFolder = "";
if (/[А-И]/.test(firstLetter)) {
    targetFolder = `${startFolder}/А-И/${firstLetter}`;
} else if (/[К-Т]/.test(firstLetter)) {
    targetFolder = `${startFolder}/К-Т/${firstLetter}`;
} else if (/[У-Я]/.test(firstLetter)) {
    targetFolder = `${startFolder}/У-Я/${firstLetter}`;
} else if (/[A-I]/.test(firstLetter)) {
    targetFolder = `${startFolder}/A-I/${firstLetter}`;
} else if (/[J-Q]/.test(firstLetter)) {
    targetFolder = `${startFolder}/J-Q/${firstLetter}`;
} else if (/[R-Z]/.test(firstLetter)) {
    targetFolder = `${startFolder}/R-Z/${firstLetter}`;
} else {
    targetFolder = `${startFolder}/-Другое`;
}

  let finalFilePath = `${targetFolder}/${fileName}`;
  let counter = 2;
  while (await app.vault.adapter.exists(`${finalFilePath}.md`)) {
    finalFilePath = `${targetFolder}/${fileName} - ${counter}`;
    counter++;
  }
  await tp.file.rename(fileName);
  await tp.file.move(finalFilePath);
%>
