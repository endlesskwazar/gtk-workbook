# GTK intro

**GTK+** (від The GIMP ToolKit) — кросплатформовий набір інструментів для створення графічних інтерфейсів користувача. Разом із Qt є одним із найпопулярніших інструментів для X Window System.

GTK+ було розроблено для GNU Image Manipulation Program (GIMP), растрового графічного редактора, у 1997 році Спенсером Кімбалом (Spencer Kimball) та Петером Матісом (Peter Mattis), членами eXperimental Computing Facility (XCF) в UC Berkeley. GTK+ розвивається в рамках проекту GNU і є вільним програмним забезпеченням.

До складу тулкіта входить повний набір віджетів, що дозволяють використовувати GTK+ для проектів різного рівня і розміру. GTK+ спеціально спроектований для підтримки не тільки C/C++, але й інших мов програмування, таких як Perl і Python, що в поєднанні з використанням візуального будівника інтерфейсу Glade дозволяє істотно спростити розробку і скоротити час написання графічних інтерфейсів.

GTK+ написана на мові програмування С, і використовує систему об'єктів GObject, що робить її об'єктно-орієнтованою. Платформа GNOME забезпечує міжмовне зв'язування для наступних мов:

- C++ (gtkmm)
- Fortran (gtk-fortran)
- Perl (Gtk2-perl)
- Ruby (ruby-gtk2)
- Python (PyGTK)
- Java (java-gnome) (не доступна Microsoft Windows)
- C# (Gtk#)
- PHP (PHP-GTK)

# GLib

**GLib** — низькорівнева бібліотека, що розширює можливості, надані стандартної бібліотекою libc мови C.

Розробляється в рамках і лежить в основах проектів GTK+ і GNOME. GLib широко використовується в застосунках, в тому числі і неграфічних. Випуск чергової версії бібліотеки за часом зазвичай збігається з випуском нової версії GTK+.

Glib надає основну об'єктну систему, яка використовується в GNOME, реалізацію основного циклу, а також великий набір допоміжних функцій для рядків і типових структур даних.

Зараз GLib здатна працювати на багатьох UNIX-подібних операційних системах, а також Windows, OS/2 і BeOS.

GLib надає такі можливості:

- великий набір базових і похідних типів даних;
- макроси і розвинений механізм налагоджувальних повідомлень;
- рядкові функції;
- функції для перетворення символів та підтримки юнікоду;
- вбудовані макроси gettext для інтернаціоналізації;
- засоби для роботи з динамічною пам'яттю;
- засоби для динамічного завантаження модулів;
- атомарні операції;
- програмні ниті і засоби їхньої синхронізації;
- породження нових процесів;
- таймери, функції для роботи з датою і часом;
- генератор псевдо-випадкових чисел;
- універсальний лексичний сканер;
- синтаксичний аналізатор параметрів командного рядка;
- синтаксичний аналізатор підмножини даних типу XML;
- синтаксичний аналізатор. ini-подібних конфігураційних файлів;
- засоби введення-виведення;
- функції перезахоплення;
- утиліти командного рядка;
- синтаксичний аналізатор файлів, що містять закладку;
- засоби роботи з регулярними виразами типу Glob.

Базові типи даних GLib призначені для зручності програміста і переносимості програми. Вони діляться на такі групи:
- Цілі типи з фіксованим розміром — gint8, guint8, gint16, guint16, gint32, guint32, gint64, guint64. Розмір змінної будь-якого з цих типів однаковий для кожної використовуваної апаратної платформи. Для gint8, наприклад, він завжди дорівнює 8 біт.
- Тезки стандартних типів мови C — gpointer (аналог void *), gconstpointer, guchar (аналог unsigned char), guint, gushort, gulong, gchar (аналог char), gint, gshort, glong, gfloat і gdouble.
- Тип gboolean зі значеннями TRUE і FALSE, типи gsize і gssize для представлення розмірів структур даних.
- Тип GString, схожий на стандартні рядок C, за винятком того, що вони автоматично розширюються, коли текст додається чи вставляється. Також, він зберігає довжину рядка, так що може бути використаний для двійкових даних з нульовими байтами.

# GObject

GObject - частина бібліотеки GLib, що реалізує об'єктів-оріентірованнние розширення для чистого C. Подібна концепція, крім самої GLib, використовується в таких проектах, як GStreamer, GSettings, ATK, Pango і весь проект GNOME в цілому, а також у великій кількості прикладних програм: GIMP, Inkscape, Geany, Gedit і багатьох інших. Велика кількість мов програмування, починаючи від таких мейнстрімових, як Python і Java, і закінчуючи шедеврами на кшталт Haskell або D, мають прив'язки до GLib / GTK +, а для значної кількості мов Біндінг до GTK + взагалі є єдиним способом побудови GUI.

На відміну від інших схожих проектів, GObject відрізняють архітектурні особливості, метою яких є легка і прозора реалізація прив'язок бібліотек, написаних із застосуванням чистого Сі і GObject, до інших мов програмування, в тому числі з динамічною типізацією і управлінням пам'яттю за допомогою збирача сміття. Саме цим пояснюється деяке відчуття переусложнённості, яке може виникнути у програміста, який приступив до знайомства з GObject API. Проте, ця система дуже продумана і логічна, так що проблем з розумінням усього викладеного нижче у програміста, знайомого з C ++ або Java, виникнути не повинно.

# Ієрархія об'єктів GTK


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

Створимо функцію on_button_click, яка буде змінювати текст кнопки:

```
void on_button_click(GtkButton *button, gpointer data)
{
    gtk_button_set_label(button, "Button was clicked");
}
```

Тепер зв'яжимо сигнал "clicked" кнопки і функцію on_button_click

```
g_signal_connect(GTK_BUTTON(button), "clicked", G_CALLBACK(on_button_click), NULL);
```

Тепер джерельний код програми наступний:

```
#include <gtk/gtk.h>

void on_button_click(GtkButton *button, gpointer data)
{
    gtk_button_set_label(button, "Button was clicked");
}

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
    g_signal_connect(GTK_BUTTON(button), "clicked", G_CALLBACK(on_button_click), NULL);

    gtk_container_add(GTK_CONTAINER(window), button);

    gtk_widget_show_all(window);

    gtk_main();
}
```

# Домашнє завдання

## Варіанти

# Контрольні запитання