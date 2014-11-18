rtz2fix
=======

Временное исправление некорректной работы объекта Date в браузерах (fix Microsoft update KB2998527 for Browsers).

Краткое описание проблемы
=========================
После выпуска упомянутого выше обновления я столкнулся со следующими проблемами:
1. В IE (Browser Mode = "Internet Explorer 10 Compat View", Document Mode = "IE8 standarts" ):
new Date(2014, 0, 1).toString() == "Tue Dec 31 23:00:00 UTC+0300 2013"
new Date(2015, 0, 7).toString() == "Tue Jan 6 23:00:00 UTC+0300 2015"
2. В Chrome 38.0.2125.122 m:
new Date(2014, 0, 1).toString() == "Wed Jan 01 2014 01:00:00 GMT+0400 (Russia TZ 2 Daylight Time)"
3. Вследствии этого перестала работать библиотека momentjs:
3.1. В IE (Browser Mode = "Internet Explorer 10 Compat View", Document Mode = "IE8 standarts" ):
  moment().startOf('year').toString() == "Tue Dec 31 2013 23:00:00 GMT+0300"
  moment().endOf('year').toString() == "Wed Dec 31 2014 22:59:59 GMT+0300"
3.2. В Chrome 38.0.2125.122 m:
  moment().startOf('year').toString() == "Wed Jan 01 2014 01:00:00 GMT+0400"
  moment().endOf('year').toString() == "Thu Jan 01 2015 00:59:59 GMT+0300"

Решение проблемы
================
Вместо того, чтобы дорабатывать библиотеки и надеясь на то, что рано или поздно проблема будет решена во всех браузерах (например, в IE Browser Mode = "Internet Explorer 10 Compat View", Document Mode = "IE10 standarts" проблемы нет), решил доработать объект Date.
Принцип такой - время в UTC+0000 непрерывно, поэтому все манипуляции с датой происходят в UTC, а timezonе добавляется в последний момент.

Буду рад, если кто-то обнаружит проблемы в данном решении и сообщит о них мне.

Использование
=============
Необходимо загрузить rtz2fix.js раньше любого другого кода, который может поменять объект Date (например, es5-shim.js).