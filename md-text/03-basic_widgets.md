# Основні користувацькі інтерфейси GTK

# Зміст

${toc}

# Label

GtkLabel - віджет, який відображає невеликий або середній обсяг тексту

Для створення віджета використовується функція - gtk_label_new.

```cpp
GtkWidget* gtk_label_new (const gchar *str);
```

В функцію можна передати NULL для створення без тексту

```cpp
#include <gtk/gtk.h>

int main (int argc, char **argv)
{
    GtkWidget* window;
    GtkWidget* label;

    gtk_init(&argc, &argv);

    //inti the window
    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Hello world");
    gtk_window_set_default_size(GTK_WINDOW(window), 500, 500);
    g_signal_connect(G_OBJECT(window), "destroy", G_CALLBACK(gtk_main_quit), NULL);


    //init the label
    label = gtk_label_new("Hello World!!!");

    //add combobox to window
    gtk_container_add(GTK_CONTAINER(window), label);

    //show the window
    gtk_widget_show_all(window);
    gtk_main();
}
```

Для позиціонування тексту існує декілька користних функцій:

**gtk_label_set_justify ()**
```cpp
void
gtk_label_set_justify (GtkLabel *label,
                       GtkJustification jtype);
```

Встановлює вирівнювання рядків у тексті відносно один одного. GTK_JUSTIFY_LEFT - це значення за промовчанням, коли віджет спочатку створюється за допомогою gtk_label_new (). gtk_label_set_justify () не впливає на мітки, які містять лише один рядок.

**gtk_label_set_xalign ()**
```cpp
void
gtk_label_set_xalign (GtkLabel *label,
                      gfloat xalign);
```

Встановлює позиціонування тексту по осі X. Значення xalign знаходиться в діапазоні 0-1.

```cpp
#include <gtk/gtk.h>

int main (int argc, char **argv)
{
    GtkWidget* window;
    GtkWidget* label;

    gtk_init(&argc, &argv);

    //inti the window
    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Hello world");
    gtk_window_set_default_size(GTK_WINDOW(window), 500, 500);
    g_signal_connect(G_OBJECT(window), "destroy", G_CALLBACK(gtk_main_quit), NULL);


    //init the label
    label = gtk_label_new("Hello World!!!");
    gtk_label_set_xalign(GTK_LABEL(label), 1);

    //add combobox to window
    gtk_container_add(GTK_CONTAINER(window), label);

    //show the window
    gtk_widget_show_all(window);
    gtk_main();
}
```

**gtk_label_set_yalign ()**

```cpp
void
gtk_label_set_yalign (GtkLabel *label,
                      gfloat yalign);
```

Встановлює позиціонування тексту по осі Y. Значення xalign знаходиться в діапазоні 0-1.

Варто, розгалядати ситуацію, коли текст не поміщається в поточні розміри віджета. Для цього існує функція gtk_label_set_line_wrap ()

```cpp
void
gtk_label_set_line_wrap (GtkLabel *label,
                         gboolean wrap);
```

Перемикає перенесення рядків у межах віджета GtkLabel. TRUE робить переривання рядків, якщо текст перевищує розмір віджета. FALSE дозволяє тексту обрізати краєм віджета, якщо він перевищує розмір віджета.

```cpp
#include <gtk/gtk.h>

int main (int argc, char **argv)
{
    GtkWidget* window;
    GtkWidget* label;

    gtk_init(&argc, &argv);

    //inti the window
    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Hello world");
    gtk_window_set_default_size(GTK_WINDOW(window), 500, 500);
    g_signal_connect(G_OBJECT(window), "destroy", G_CALLBACK(gtk_main_quit), NULL);


    //init the label
    label = gtk_label_new("Toggles line wrapping within"
                          " the GtkLabel widget. TRUE makes it break lines"
                          " if text exceeds the widget’s size. FALSE lets"
                          " the text get cut off by the edge of the widget "
                          "if it exceeds the widget size.");
    gtk_label_set_line_wrap(GTK_LABEL(label), TRUE);

    //add combobox to window
    gtk_container_add(GTK_CONTAINER(window), label);

    //show the window
    gtk_widget_show_all(window);
    gtk_main();
}
```

Зверніть увагу, на те, що текст не можна виділити і скопіювати за допомогою мишки. Щоб це виправити можна використати функцію: **gtk_label_set_selectable ()**

```cpp
void
gtk_label_set_selectable (GtkLabel *label,
                          gboolean setting);
```

Selectable lable дозволяють користувачеві вибирати текст з мітки, для копіювання та вставки.

```cpp
#include <gtk/gtk.h>

int main (int argc, char **argv)
{
    GtkWidget* window;
    GtkWidget* label;

    gtk_init(&argc, &argv);

    //inti the window
    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Hello world");
    gtk_window_set_default_size(GTK_WINDOW(window), 500, 500);
    g_signal_connect(G_OBJECT(window), "destroy", G_CALLBACK(gtk_main_quit), NULL);


    //init the label
    label = gtk_label_new("Toggles line wrapping within"
                          " the GtkLabel widget. TRUE makes it break lines"
                          " if text exceeds the widget’s size. FALSE lets"
                          " the text get cut off by the edge of the widget "
                          "if it exceeds the widget size.");
    gtk_label_set_line_wrap(GTK_LABEL(label), TRUE);

    //add combobox to window
    gtk_container_add(GTK_CONTAINER(window), label);

    //show the window
    gtk_widget_show_all(window);
    gtk_main();
}
```

# GtkButton

**GtkButton** - віджет, який видає сигнал при натисканні.

Створити кнопку можна, використовуючи наступні функції:

- **gtk_button_new** - Створює пусту кнопку
- **gtk_button_new_with_label** - Створює кнопку з переданим текстом

```cpp
#include <gtk/gtk.h>

int main (int argc, char **argv)
{
    GtkWidget* window;
    GtkWidget* button;

    gtk_init(&argc, &argv);

    //inti the window
    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Hello world");
    gtk_window_set_default_size(GTK_WINDOW(window), 500, 500);
    g_signal_connect(G_OBJECT(window), "destroy", G_CALLBACK(gtk_main_quit), NULL);


    //init the button
    button = gtk_button_new_with_label("Click");

    //add button to window
    gtk_container_add(GTK_CONTAINER(window), button);

    //show the window
    gtk_widget_show_all(window);
    gtk_main();
}
```

## GtkButton в якості контейнера

GtkButton може виступати в якості контейнера для інших віджетів. Давайте додамо да GtkButton інший віджет по аналогії як ми додали GtkButton в GtkWindow

```cpp
#include <gtk/gtk.h>

int main (int argc, char **argv)
{
    GtkWidget* window;
    GtkWidget* button;
    GtkWidget* lable;

    gtk_init(&argc, &argv);

    //inti the window
    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Hello world");
    gtk_window_set_default_size(GTK_WINDOW(window), 500, 500);
    g_signal_connect(G_OBJECT(window), "destroy", G_CALLBACK(gtk_main_quit), NULL);

    //init the lable
    lable = gtk_label_new("Click");

    //init the button
    button = gtk_button_new();

    //add lable to button
    gtk_container_add(GTK_CONTAINER(button), lable);

    //add button to window
    gtk_container_add(GTK_CONTAINER(window), button);

    //show the window
    gtk_widget_show_all(window);
    gtk_main();
}
```

## Зміна надпису Lable, при натиску на кнопку

Одним із основних сигналів є **clicked**:

```cpp
#include <gtk/gtk.h>

GtkWidget* lable;

void on_button_clicked(GtkButton* button, gpointer data)
{
    gtk_label_set_text(GTK_LABEL(lable), "it was clicked");
}

int main (int argc, char **argv)
{
    GtkWidget* window;
    GtkWidget* button;

    gtk_init(&argc, &argv);

    //inti the window
    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Hello world");
    gtk_window_set_default_size(GTK_WINDOW(window), 500, 500);
    g_signal_connect(G_OBJECT(window), "destroy", G_CALLBACK(gtk_main_quit), NULL);

    //init the lable
    lable = gtk_label_new("Click");

    //init the button
    button = gtk_button_new();
    g_signal_connect(G_OBJECT(button), "clicked", G_CALLBACK(on_button_clicked), NULL);

    //add lable to button
    gtk_container_add(GTK_CONTAINER(button), lable);

    //add button to window
    gtk_container_add(GTK_CONTAINER(window), button);

    //show the window
    gtk_widget_show_all(window);
    gtk_main();
}
```

## Зміна активності кнопки



# ComboBox

**GtkComboBox** - віджет, який використовується для вибору зі списку елементів.

Для створення путих списків можна використати функції:
- gtk_combo_box_new ()
- gtk_combo_box_new_with_entry ()

```cpp
#include <gtk/gtk.h>

int main (int argc, char **argv)
{
    GtkWidget* window;
    GtkWidget* comboBox;

    gtk_init(&argc, &argv);

    //inti the window
    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Hello world");
    gtk_window_set_default_size(GTK_WINDOW(window), 500, 500);
    g_signal_connect(G_OBJECT(window), "destroy", G_CALLBACK(gtk_main_quit), NULL);


    //init the combobox
    comboBox = gtk_combo_box_new();

    //add combobox to window
    gtk_container_add(GTK_CONTAINER(window), comboBox);

    //show the window
    gtk_widget_show_all(window);
    gtk_main();
}
```