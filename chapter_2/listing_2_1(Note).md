"Сопрограмму(корутину) можно рассматривать" как обычную функцию Python, наделенную сверхспособностью: приостанавливаться, встретив операцию, для выполнения которой нужно заметное время. По завершении такой длительной операции сопрограмму можно «пробудить», после чего она продолжит выполнение. Пока приостановленная сопрограмма ждет завершения операции, мы можем выполнять другой код. Такое выполнение другого кода во время ожидания и обеспечивает конкурентность внутри приложения. Сопрограммы не выполняются, если их вызвать напрямую. Вместо этого возвращается объект сопрограммы, который будет выполнен позже. Чтобы выполнить сопрограмму, мы должны явно передать ее циклу событий.

"async" определяет сопрограмму(корутину)
"await" приостанавливает ее(сопрограмму) на время выполнения длительной операции. слово await, за ним обычно следует обращение к сопрограмме (точнее, к объекту, допускающему ожидание, который необязательно является сопрограммой). Использование ключевого слова await приводит к  выполнению следующей за ним сопрограммы

"asyncio.run" - функция задумана как главная точка входа в созданное нами приложение asyncio.
                Делает несколько важных вещей.
                    1) она создает новое событие.
                    2) выполняет код переданной нами сопрограммы до конца и возвращает результат.
                    3) подчищает все то, что могло остаться после завершения сопрограммы.
                    4) в конце она останавливает и закрывает цикл событий

"asyncio.sleep()" сама является сопрограммой, поэтому вызывать ее следует с помощью await. Вызвав ее напрямую, мы получим просто объект сопрограммы. Раз asyncio.sleep – сопрограмма, то, пока мы ее ждем, может выполняться другой код.


# Сопрограмма, которую выполняет asyncio.run, должна создать и запустить все прочие сопрограммы, это позволит нам обратить себе на пользу конкурентную природу asyncio

достоинство asyncio – возможность приостановить(await) выполнение и дать циклу событий возможность выполнить другие задачи, пока длительная операция(сопрограммы) делает свое дело.

"Задача" – это обертка вокруг сопрограммы, которая планирует выполнение последней в цикле событий как можно раньше. И планирование, и выполнение происходят в неблокирующем режиме, т. е., создав задачу, мы можем сразу приступить к выполнению другого кода, пока эта задача работает в фоне.

"Снятие задач и задание тайм-аутов" - отправляя запросы, мы должны внимательно следить за ними, чтобы не ждать слишком долго. Иначе приложение может зависнуть, ожидая результата, который никогда не придет.

"Task.cancel()" - Вызов cancel не прерывает задачу, делающую свое дело; он снимает ее. Код будет продолжать работать, пока не встретится следующее предложение await. И вызовит исключение asyncio.exceptions.CancelledError

"asyncio.wait_for(Task, timeout)" - задает таймаут работы функции, по истечении указанного времени будет вызвано исключение TimeoutError

"asyncio.shield(Task)" - защита от снятия нужна для того чтоб отловить задачу(future) в блоке except.

# И сопрограммы, и задачи можно использовать в выражениях await. Так что же между ними общего?


"Future" - это клас обертка над корутиной, которую тоже можно использовать в выражения await. По началу он пустой
Future методы: done() - проверка установлен ли результат
               set_result()