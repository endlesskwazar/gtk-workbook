# Drawing area, key events

# Зміст

${toc}

# Key events

## GDK

**GDK** - це бібліотека низького рівня, яку використовує GTK + для взаємодії з віконною системою для графіки та пристроїв введення. Хоча GDK рідко використовується безпосередньо в коді програми, він містить всі необхідні функції для створення вікон низького рівня на екрані та взаємодії з користувачем з різними пристроями введення. GDK діє як абстракція над різними віконними системами, так що GTK + може бути портативним для всіх: X Window System (X11), Microsoft Windows, Mac OS X.

## Acselerators

## Custom key handling

Для прослуховування подій певним віджетом потрібно скористатися функцією:
```cpp
void
gtk_widget_add_events (GtkWidget *widget,
                       gint events);
```

- віджет, який буде слухати подію
- event mask

enum GdkEventType:

|Значення|Пояснення|
|-|-|
|GDK_BUTTON_PRESS_MASK|отримувати події натискання кнопок(мишки)|
|GDK_BUTTON_RELEASE_MASK|події віджимання кнопки(мишки)|
|GDK_KEY_PRESS_MASK|отримувати події натиску на клавіші|
|GDK_KEY_RELEASE_MASK|отримувати події віджимання клавіші|
|GDK_ALL_EVENTS_MASK|поєднання всіх масок подій.|

Повний список: [gdk3-Events](https://developer.gnome.org/gdk3/stable/gdk3-Events.html)

### Приклад подій із мишкою

Натиснута або відпущена кнопка, пронумерована від 1 до 5. Зазвичай кнопка 1 - ліва кнопка миші, 2 - середня, а 3 - права.

```cpp
#include <cairo.h>
#include <gtk/gtk.h>

void mouse_clicked(GtkWidget *widget, GdkEventButton *event,
                   gpointer data){
    if(event->button == 1){
        g_print("left mouse clicked");
    }
    if(event->button == 3){
        g_print("right mouse clicked");
    }

}

int main(int argc, char *argv[])
{
    GtkWidget *window;


    gtk_init(&argc, &argv);

    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);

    gtk_widget_add_events(window, GDK_BUTTON_PRESS_MASK);

    g_signal_connect(window, "destroy",
                     G_CALLBACK(gtk_main_quit), NULL);

    g_signal_connect(window, "button-press-event",
                     G_CALLBACK(mouse_clicked), NULL);

    gtk_window_set_position(GTK_WINDOW(window), GTK_WIN_POS_CENTER);
    gtk_window_set_default_size(GTK_WINDOW(window), 400, 300);
    gtk_window_set_title(GTK_WINDOW(window), "Events");

    gtk_widget_show_all(window);

    gtk_main();

    return 0;
}
```

### Визначення координат курсора, при клікові

```cpp
#include <cairo.h>
#include <gtk/gtk.h>

void mouse_clicked(GtkWidget *widget, GdkEventButton *event,
                   gpointer data){
    if(event->button == 1){
        g_print("left mouse clicked");
        gchar buf[5];
        gchar* x = g_ascii_dtostr(buf, 5, event->x);
        g_print(x);
    }
    if(event->button == 3){
        g_print("right mouse clicked");
    }

}

int main(int argc, char *argv[])
{
    GtkWidget *window;


    gtk_init(&argc, &argv);

    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);

    gtk_widget_add_events(window, GDK_BUTTON_PRESS_MASK);

    g_signal_connect(window, "destroy",
                     G_CALLBACK(gtk_main_quit), NULL);

    g_signal_connect(window, "button-press-event",
                     G_CALLBACK(mouse_clicked), NULL);

    gtk_window_set_position(GTK_WINDOW(window), GTK_WIN_POS_CENTER);
    gtk_window_set_default_size(GTK_WINDOW(window), 400, 300);
    gtk_window_set_title(GTK_WINDOW(window), "Events");

    gtk_widget_show_all(window);

    gtk_main();

    return 0;
}
```

### Приклад подій із клавіатурою

```cpp
#include <gtk/gtk.h>

void mouse_clicked(GtkWidget *widget, GdkEventKey *event,
                   gpointer data){
    if(event->keyval == GDK_KEY_q || event->keyval == GDK_KEY_Q){
        g_print("q was presed");
    }
    if(event->keyval == GDK_KEY_x){
        g_print("x was pressed");
    }

}

int main(int argc, char *argv[])
{
    GtkWidget *window;


    gtk_init(&argc, &argv);

    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);

    gtk_widget_add_events(window, GDK_BUTTON_PRESS_MASK);
    gtk_widget_add_events(window, GDK_KEY_PRESS_MASK);

    g_signal_connect(window, "destroy",
                     G_CALLBACK(gtk_main_quit), NULL);

    g_signal_connect(window, "key-press-event",
                     G_CALLBACK(mouse_clicked), NULL);

    gtk_window_set_position(GTK_WINDOW(window), GTK_WIN_POS_CENTER);
    gtk_window_set_default_size(GTK_WINDOW(window), 400, 300);
    gtk_window_set_title(GTK_WINDOW(window), "Events");

    gtk_widget_show_all(window);

    gtk_main();

    return 0;
}
```

### Пропагінація подій

```cpp

```


# Drawing area & Cairo

## Cairo

Cairo - це програмна бібліотека для малювання векторної графіки з відкритим джерельним кодом. Включає в себе апаратно-незалежний прикладний програмний інтерфейс для розробників програмного забезпечення. Cairo надає графічні примітиви для відтворення двовимірних зображень за допомогою різноманітних бекендов. Коли є можливість, Cairo використовує апаратне прискорення.

## Drawing Area

DrawingArea по суті є вікном X і нічого більше. Це чисте полотно, в якому ми можемо намалювати все, що нам подобається. Drawing
area створюється за допомогою виклику:

```cpp
GtkWidget* gtk_drawing_area_new(void);
```

Розмір за замовчуванням для віджета можна вказати за допомогою виклику:

```cpp
void       gtk_drawing_area_size       (GtkDrawingArea *darea, gint width, gint height);
```

Розмір за замовчуванням може бути перевизначений, як це справедливо для всіх віджетів, за допомогою виклику:

```cpp
gtk_widget_set_size_request ()
```

### Малювання ліній(приклад)

```cpp
#include <cairo.h>
#include <gtk/gtk.h>

gboolean on_draw_event(GtkWidget *widget, cairo_t *cr,
                              gpointer user_data)
{
    cairo_set_source_rgb(cr, 0, 0, 0);
    cairo_set_line_width(cr, 0.5);
    cairo_move_to(cr, 50, 50);
    cairo_line_to(cr, 100, 100);
    cairo_stroke(cr);
    gtk_widget_queue_draw(widget);

    //g_print("drawing...");

    return FALSE;
}


int main(int argc, char *argv[])
{
    GtkWidget *window;
    GtkWidget *darea;

    gtk_init(&argc, &argv);

    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);

    darea = gtk_drawing_area_new();
    gtk_container_add(GTK_CONTAINER(window), darea);

    gtk_widget_add_events(window, GDK_BUTTON_PRESS_MASK);

    g_signal_connect(G_OBJECT(darea), "draw",
                     G_CALLBACK(on_draw_event), NULL);
    g_signal_connect(window, "destroy",
                     G_CALLBACK(gtk_main_quit), NULL);

    gtk_window_set_position(GTK_WINDOW(window), GTK_WIN_POS_CENTER);
    gtk_window_set_default_size(GTK_WINDOW(window), 400, 300);
    gtk_window_set_title(GTK_WINDOW(window), "Lines");

    gtk_widget_show_all(window);

    gtk_main();

    return 0;
}
```

**cairo_t** є основним об'єктом, що використовується при малюванні з cairo. Щоб намалювати за допомогою cairo, ви створюєте cairo_t, встановлюєте surface та параметри малювання для cairo_t, створюєте фігури з функціями, такими як:
- cairo_move_to - Початок нового шляху. Після цього виклику поточна точка буде (x, y).
- cairo_line_to () - Додає рядок до шляху від поточної точки до позиції (x, y) у координатах простору користувача. Після цього виклику поточна точка буде (x, y).



## Малювання ліній, використовуючи мишку

```cpp
#include <cairo.h>
#include <gtk/gtk.h>

void do_drawing(cairo_t *);

struct {
    int count;
    double coordx[100];
    double coordy[100];
} glob;

gboolean on_draw_event(GtkWidget *widget, cairo_t *cr,
                              gpointer user_data)
{
    do_drawing(cr);

    return FALSE;
}

void do_drawing(cairo_t *cr)
{
    cairo_set_source_rgb(cr, 0, 0, 0);
    cairo_set_line_width(cr, 0.5);

    int i, j;
    for (i = 0; i <= glob.count - 1; i++ ) {
        for (j = 0; j <= glob.count - 1; j++ ) {
            cairo_move_to(cr, glob.coordx[i], glob.coordy[i]);
            cairo_line_to(cr, glob.coordx[j], glob.coordy[j]);
        }
    }

    glob.count = 0;
    cairo_stroke(cr);
}

gboolean clicked(GtkWidget *widget, GdkEventButton *event,
                        gpointer user_data)
{
    if (event->button == 1) {
        glob.coordx[glob.count] = event->x;
        glob.coordy[glob.count++] = event->y;
    }

    if (event->button == 3) {
        gtk_widget_queue_draw(widget);
    }

    return TRUE;
}


int main(int argc, char *argv[])
{
    GtkWidget *window;
    GtkWidget *darea;

    glob.count = 0;

    gtk_init(&argc, &argv);

    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);

    darea = gtk_drawing_area_new();
    gtk_container_add(GTK_CONTAINER(window), darea);

    gtk_widget_add_events(window, GDK_BUTTON_PRESS_MASK);

    g_signal_connect(G_OBJECT(darea), "draw",
                     G_CALLBACK(on_draw_event), NULL);
    g_signal_connect(window, "destroy",
                     G_CALLBACK(gtk_main_quit), NULL);

    g_signal_connect(window, "button-press-event",
                     G_CALLBACK(clicked), NULL);

    gtk_window_set_position(GTK_WINDOW(window), GTK_WIN_POS_CENTER);
    gtk_window_set_default_size(GTK_WINDOW(window), 400, 300);
    gtk_window_set_title(GTK_WINDOW(window), "Lines");

    gtk_widget_show_all(window);

    gtk_main();

    return 0;
}
```

У нашому прикладі ми натискаємо випадково на вікно за допомогою лівої кнопки миші. Кожен клік зберігається в масиві. Коли ми натискаємо правою кнопкою миші на вікно, всі точки з'єднуються з кожною точкою масиву. Таким чином, ми можемо створити кілька цікавих об'єктів. Клацніть правою кнопкою миші на намальованому об'єкті, очищуючи вікно, і ми можемо намалювати інший об'єкт.

1. Лінії будуть намальовані чорним кольром і будуть мати товщину 0,5.
```cpp
cairo_set_source_rgb(cr, 0, 0, 0);
cairo_set_line_width (cr, 0.5);
```

2. Ми з'єднуємо кожну точку з масиву з кожною іншою точкою.
```cpp
int i, j;
for (i = 0; i <= glob.count - 1; i++ ) {
    for (j = 0; j <= glob.count - 1; j++ ) {
        cairo_move_to(cr, glob.coordx[i], glob.coordy[i]);
        cairo_line_to(cr, glob.coordx[j], glob.coordy[j]);
    }
}
```

3. Підключаємо button-press-event функції clicked.
```cpp
g_signal_connect(window, "button-press-event", 
    G_CALLBACK(clicked), NULL);
```

Усередині clicked, ми визначаємо, чи є подія лівою або правою кнопкою миші. Якщо ми натискаємо лівою кнопкою миші, зберігаємо координати x, y у масивах. Правою кнопкою ми перемальовуємо вікно.

## Basic shapes


## Fill & Stroke

## Paint

# Домашнє завдання

# Контрольні запитання