# Как написать своего клиента
Необходимо подключиться из кода к серверу через вебсокеты.
ws://server_ip:8080/codenjoy-contest/ws?user=email&pwd=password
- email - имейл, указанынй при регистрации на сервере.
- password - указанный при регистрации и закодирован в md5

После подключения клиент будет регулярно (каждую секунду) получать строку символов — с закодированным состоянием поля. 
С помощью этого regexp можно выкусить строку доски:
```
^board=(.*)$
```
Пример строки от сервера:

```
board=☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼ #   # #  #♥#  #  #  &        #☼☼♥☼♥☼♥☼#☼ ☼ ☼ ☼ ☼♥☼ ☼ ☼#☼#☼♥☼#☼#☼☼#♥♥  ♥#   # #♥   # ♥#          ☼☼ ☼ ☼#☼ ☼♥☼ ☼ ☼#☼ ☼ ☼ ☼ ☼&☼ ☼ ☼ ☼☼     ♥          # #            ☼☼ ☼ ☼ ☼ ☼♥☼ ☼ ☼♥☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼☼#       # #       ☺& 2  #  #  #☼☼#☼♥☼ ☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼ ☼ ☼☼#  # ♥#               # ♥   #  ☼☼ ☼ ☼#☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼☼   #♥ #      #                 ☼☼ ☼ ☼ ☼ ☼♥☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼☼     ## #     #   # #   ♥      ☼☼ ☼ ☼♥☼ ☼ ☼#☼ ☼#☼ ☼ ☼♥☼ ☼ ☼ ☼ ☼ ☼☼       #♥       #      ## # ###☼☼ ☼ ☼ ☼#☼ ☼ ☼#☼ ☼ ☼#☼#☼&☼ ☼ ☼ ☼ ☼☼       #       #    ♣# #     ♥ ☼☼ ☼ ☼ ☼♥☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼☼        ## ## ♥             # #☼☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼☼                   &    ###  ##☼☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼☼                   ♥ ##        ☼☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼♥☼#☼ ☼ ☼ ☼☼     ##         &#         #   ☼☼ ☼ ☼ ☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼ ☼ ☼ ☼☼   #   #         #     # &     ☼☼♥☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼#☼ ☼☼  #                    ##   &  ☼☼ ☼ ☼ ☼ ☼ ☼#☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼#☼ ☼☼ #    # &        #       #     ☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼
```
Длина строки без "board=" равна площади поля. Если вставить символ переноса строки каждые sqrt(length(string)) символов, то получится читабельное изображение поля.
```
☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼
☼ #   # #  #♥#  #  #  &        #☼
☼♥☼♥☼♥☼#☼ ☼ ☼ ☼ ☼♥☼ ☼ ☼#☼#☼♥☼#☼#☼
☼#♥♥  ♥#   # #♥   # ♥#          ☼
☼ ☼ ☼#☼ ☼♥☼ ☼ ☼#☼ ☼ ☼ ☼ ☼&☼ ☼ ☼ ☼
☼     ♥          # #            ☼
☼ ☼ ☼ ☼ ☼♥☼ ☼ ☼♥☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼
☼#       # #       ☺& 2  #  #  #☼
☼#☼♥☼ ☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼ ☼ ☼
☼#  # ♥#               # ♥   #  ☼
☼ ☼ ☼#☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼
☼   #♥ #      #                 ☼
☼ ☼ ☼ ☼ ☼♥☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼
☼     ## #     #   # #   ♥      ☼
☼ ☼ ☼♥☼ ☼ ☼#☼ ☼#☼ ☼ ☼♥☼ ☼ ☼ ☼ ☼ ☼
☼       #♥       #      ## # ###☼
☼ ☼ ☼ ☼#☼ ☼ ☼#☼ ☼ ☼#☼#☼&☼ ☼ ☼ ☼ ☼
☼       #       #    ♣# #     ♥ ☼
☼ ☼ ☼ ☼♥☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼
☼        ## ## ♥             # #☼
☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼
☼                   &    ###  ##☼
☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼
☼                   ♥ ##        ☼
☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼♥☼#☼ ☼ ☼ ☼
☼     ##         &#         #   ☼
☼ ☼ ☼ ☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼ ☼ ☼ ☼
☼   #   #         #     # &     ☼
☼♥☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼#☼ ☼
☼  #                    ##   &  ☼
☼ ☼ ☼ ☼ ☼ ☼#☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼#☼ ☼
☼ #    # &        #       #     ☼
☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼
```
Первый символ строки соответствует ячейке расположенной в левом верхнем углу и имеет координату [0, 0]. 
В этом примере — позиция бомбермена (символ ☺) — [19,7].

Расшифровка символов на рисунке ниже

```public enum Element {

    /// This is your Bomberman
    BOMBERMAN('☺'),             // this is what he usually looks like
    BOMB_BOMBERMAN('☻'),        // this is if he is sitting on own bomb
    DEAD_BOMBERMAN('Ѡ'),        // oops, your Bomberman is dead (don't worry, he will appear
                                // somewhere in next move)
                                // you're getting -200 for each death

    /// this is other players Bombermans
    OTHER_BOMBERMAN('♥'),       // this is what other Bombermans looks like
    OTHER_BOMB_BOMBERMAN('♠'),  // this is if player just set the bomb
    OTHER_DEAD_BOMBERMAN('♣'),  // enemy corpse (it will disappear shortly, right on the next move)
                                // if you've done it you'll get +1000

    /// the bombs
    BOMB_TIMER_5('5'),          // after bomberman set the bomb, the timer starts (5 tacts)
    BOMB_TIMER_4('4'),          // this will blow up after 4 tacts
    BOMB_TIMER_3('3'),          // this after 3
    BOMB_TIMER_2('2'),          // two
    BOMB_TIMER_1('1'),          // one
    BOOM('҉'),                  // Boom! this is what is bomb does, everything that is destroyable
                                // got destroyed

    /// walls
    WALL('☼'),                  // indestructible wall - it will not fall from bomb
    DESTROY_WALL('#'),          // this wall could be blowed up
    DESTROYED_WALL('H'),        // this is how broken wall looks like, it will dissapear on next move
                                // if it's you did it - you'll get +10 points.

    /// meatchoppers
    MEAT_CHOPPER('&'),          // this guys runs over the board randomly and gets in the way all the time
                                // if it will touch bomberman - it will die
                                // you'd better kill this piece of ... meat, you'll get +100 point for it
    DEAD_MEAT_CHOPPER('x'),     // this is chopper corpse

    /// a void
    NONE(' ');                 // this is the only place where you can move your Bomberman
```
Игра пошаговая, каждую секунду сервер посылает твоему клиенту (боту) состояние обновленного поля на текущий момент и ожидает ответ. 
За следующую секунду игрок должен успеть дать команду герою. Если не успел — герой стоит на месте.

Команд несколько: 
- Команды которые приводят к движению героя на 1 клетку: UP, DOWN, LEFT, RIGHT 
- Оставить бомбу на месте героя: ACT 

Команды движения можно комбинировать с командой ACT, разделяя их через запятую. 
Порядок (LEFT, ACT) или (ACT, LEFT) - имеет значение, либо двигаемся влево и там ставим бомбу, либо ставим бомбу а затем двигаемся влево. 
Если игрок будет использовать только одну команду ACT, то бомба установится под героем без его перемещения на поле.
