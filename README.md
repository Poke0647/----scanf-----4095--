# Как заставить scanf() считывать более 4095 символов
## Надо перевести терминал в неканоничный режим
В каноничном терминал имеет ограничение при котором входные буферизуются в очереди ввода-вывода ядра.
## Сделать это можно с помощью низкоуровневой библиотеки **<termios.h>**
Чтобы перевести терминал в неканоничный режим, можно использовать следующую функцию
```C
#include <termios.h>
#include <unistd.h>

void clear_icanon(void) // перевод терминала в неканоничный режим
{
  struct termios settings;

  if (tcgetattr (STDIN_FILENO, &settings) < 0) {
      perror ("tcgetattr");
      exit(1);
    }

  settings.c_lflag &= ~ICANON;

  if (tcsetattr (STDIN_FILENO, TCSANOW, &settings) < 0) {
      perror ("tcsetattr");
      exit(1);
   }
}
```
Затем надо вставить функцию **clear_icon();** в код.
