# GTK intro

# Налаштування середовища розробки

# Перша програма

```
#include <gtk/gtk.h>

int main (int argc, char **argv)
{
  GtkWidget *window;

  gtk_init(&argc, &argv);

  window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
  gtk_window_set_title(GTK_WINDOW(window), "Hello world");
  gtk_window_set_default_size(GTK_WINDOW(window), 500, 500);

  gtk_widget_show_all(window);

  gtk_main();
}
```

![](../resources/img/1-gtk-img-1.png)

# Сигнали

Ми можемо пов'язати певну подію, яка повинна відбутися з віджетом, зі своєю функцією.

```
g_signal_connect(G_OBJECT(window), "destroy", G_CALLBACK(gtk_main_quit), NULL);
```

В даному випадку це стандартна фунция gtk_main_quit (), яка безпечно завершить наш додаток.

```
#include <gtk/gtk.h>

int main (int argc, char **argv)
{
  GtkWidget *window;


  gtk_init(&argc, &argv);

  window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
  gtk_window_set_title(GTK_WINDOW(window), "Hello world");
  gtk_window_set_default_size(GTK_WINDOW(window), 500, 500);
  g_signal_connect(G_OBJECT(window), "destroy", G_CALLBACK(gtk_main_quit), NULL);

  gtk_widget_show_all(window);

  gtk_main();
}
```

Тепер створимо кнопку, після натискання на яку надпис на кнопці буде змінюватися.

Для початку додамо кнопку:

```
GtkWidget *button;
button = gtk_button_new_with_label("Click Me!!!");

gtk_container_add(GTK_CONTAINER(window), button);
```

Весь код програми:

```
#include <gtk/gtk.h>

int main (int argc, char **argv)
{
  GtkWidget *window;


  gtk_init(&argc, &argv);

  window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
  gtk_window_set_title(GTK_WINDOW(window), "Hello world");
  gtk_window_set_default_size(GTK_WINDOW(window), 500, 500);
  g_signal_connect(G_OBJECT(window), "destroy", G_CALLBACK(gtk_main_quit), NULL);

  GtkWidget *button;
  button = gtk_button_new_with_label("Click Me!!!");

  gtk_container_add(GTK_CONTAINER(window), button);

  gtk_widget_show_all(window);

  gtk_main();
}
```

В Результаті ми отримаємо кнопку, яка займає весь доступний простір всередині вікна.

![](../resources/img/1-gtk-img-2.png)

Покищо, при натиску на кнопку нічого не відбувається. Для того, щоб обробляти користувацькі події нам знадобиться:
- створити функцію, яка обробляє певну подію
- зв'язати подію і створену функцію

