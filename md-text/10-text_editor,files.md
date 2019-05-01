# Working with files, text editor app

# Зміст

${toc}

# GFile

**GFile** - об'єкт, який використовується для роботи із директоріями і файлами.

## Створення GFile

- **g_file_new_for_path ()**

```cpp
GFile *
g_file_new_for_path (const char *path);
```

Створює GFile для заданого шляху. Ця операція ніколи не повертає помилку, але повернутий об'єкт може не підтримувати будь-яку операцію введення-виведення, якщо шлях неправильний.

## Створення нового файла і запис в нього

Створити новий файл можна за допомогою функцій:

**g_file_create ()**:
Створює новий файл і повертає вихідний потік для запису до нього. Файл не повинен вже існувати.
```cpp
GFileOutputStream *
g_file_create (GFile *file,
               GFileCreateFlags flags,
               GCancellable *cancellable,
               GError **error);
```

Параметри:
- file - екземпляр GFile
- flags - флаги(G_FILE_CREATE_NONE, G_FILE_CREATE_PRIVATE, G_FILE_CREATE_REPLACE_DESTINATION)
- cancellable - опціональний GCancellable, NULL щоб ігнорувати. 
- error - GError, обо NULL

**g_file_replace ()**
Повертає вихідний потік для перезапису файлу, можливо, спочатку створивши резервну копію файлу. Якщо файл не існує, він буде створений.
```cpp
GFileOutputStream *
g_file_replace (GFile *file,
                const char *etag,
                gboolean make_backup,
                GFileCreateFlags flags,
                GCancellable *cancellable,
                GError **error);
```

Параметри:
- file - екземпляр GFile
- etag - опціональний ентіті тег, або NULL. 
- make_backup - TRUE, якщо створювати бекап
- flags - флаги(G_FILE_CREATE_NONE, G_FILE_CREATE_PRIVATE, G_FILE_CREATE_REPLACE_DESTINATION)
- cancellable - опціональний GCancellable, NULL щоб ігнорувати. 
- error - GError, обо NULL

**g_file_append_to ()**
Отримує вихідний потік для додавання даних до файлу. Якщо файл ще не існує, він створюється.
```cpp
GFileOutputStream *
g_file_append_to (GFile *file,
                  GFileCreateFlags flags,
                  GCancellable *cancellable,
                  GError **error);
```

Параметри:
- file - екземпляр GFile
- flags - флаги(G_FILE_CREATE_NONE, G_FILE_CREATE_PRIVATE, G_FILE_CREATE_REPLACE_DESTINATION)
- cancellable - опціональний GCancellable, NULL щоб ігнорувати. 
- error - GError, обо NULL

### Запис в файл, використовуючи GFileOutputStream

Маючи GFileOutputStream, в файл можна записати дані, використовуючи функцію:
```cpp
gssize
g_output_stream_write (GOutputStream *stream,
                       const void *buffer,
                       gsize count,
                       GCancellable *cancellable,
                       GError **error);
```

Параметри:
- stream - GOutputStream.
- buffer - буфер для запису
- count - кількість байт для запису
- cancellable - опціональний GCancellable, NULL щоб ігнорувати. 
- error - GError, обо NULL


Після не забудь закрити стрим:

```cpp
gboolean
g_output_stream_close (GOutputStream *stream,
                       GCancellable *cancellable,
                       GError **error);
```

Приклад:

```cpp
#include <gtk/gtk.h>

int main(int argc, char *argv[])
{
    gtk_init(&argc,&argv);

    GFile *file=g_file_new_for_path("test.txt");
    GFileOutputStream *output=g_file_create(
            file,
            G_FILE_CREATE_NONE,
            NULL,
            NULL);
    gint a=5;
    gfloat  b=3.14;
    /*
     * Функция g_strdup_printf идентична printf(), за исключением того, что вывод производится в массив
     */
    gchar *buf=g_strdup_printf(" %d %f",a,b);
    g_output_stream_write(G_OUTPUT_STREAM(output),
                          buf,strlen(buf),NULL,NULL);

    g_output_stream_close(G_OUTPUT_STREAM(output),NULL,NULL);
    g_free(buf);
    g_object_unref(output);
    g_object_unref(file);
    return 0;
}
```

## Відкриття файла

Для зчитання вмісту із файла можна використати функцію:
**g_file_read ()**
Відкриває файл для читання. Результатом є GFileInputStream, який можна використовувати для читання вмісту файлу.
```cpp
GFileInputStream *
g_file_read (GFile *file,
             GCancellable *cancellable,
             GError **error);
```

Параметри:
- file - екземпляр GFile
- cancellable - опціональний GCancellable, NULL щоб ігнорувати. 
- error - GError, обо NULL

### Читання вмісту із GInputStream

Для читання із GInputStream можна використати функцію:
**g_input_stream_read ()**
Спробує прочитати count байтів з потоку в буфер, починаючи з буфера. Буде блокувати під час цього читання.
```cpp
gssize
g_input_stream_read (GInputStream *stream,
                     void *buffer,
                     gsize count,
                     GCancellable *cancellable,
                     GError **error);
```

Параметри:
- stream - GInputStream
- buffer - Буфер в який буде записано дані
- count - кількість байт для читання
- cancellable - опціональний GCancellable, NULL щоб ігнорувати. 
- error - GError, обо NULL

Приклад:
```cpp
#include <gtk/gtk.h>

int main(int argc, char *argv[])
{
    gtk_init(&argc,&argv);

    GFile *file=g_file_new_for_path("test.txt");
    GFileInputStream *input=g_file_read(
            file,
            NULL,
            NULL);
    const gsize count =  10000;
    gchar buf[1000];
    g_input_stream_read(G_INPUT_STREAM(input), buf, count, NULL, NULL
            );
    g_print(buf);
    g_input_stream_close(G_INPUT_STREAM(input),NULL,NULL);
    g_free(buf);
    g_object_unref(input);
    g_object_unref(file);
    return 0;
}
```

## Відкриття дфайла з GtkFileChooserDialog

```cpp
#include <gtk/gtk.h>

int main(int argc, char *argv[])
{
    GtkWidget *window;

    gtk_init(&argc,&argv);

    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Hello world");
    gtk_window_set_default_size(GTK_WINDOW(window), 500, 500);
    g_signal_connect(G_OBJECT(window), "destroy", G_CALLBACK(gtk_main_quit), NULL);

    gtk_widget_show_all(window);

    GtkWidget *dialog;
    GtkFileChooserAction action = GTK_FILE_CHOOSER_ACTION_OPEN;
    gint res;

    dialog = gtk_file_chooser_dialog_new ("Open File",
                                          GTK_WINDOW(window),
                                          action,
                                          "Cancel",
                                          GTK_RESPONSE_CANCEL,
                                          "Open",
                                          GTK_RESPONSE_ACCEPT,
                                          NULL);

    res = gtk_dialog_run (GTK_DIALOG (dialog));
    if (res == GTK_RESPONSE_ACCEPT)
    {
        char *filename;
        GtkFileChooser *chooser = GTK_FILE_CHOOSER (dialog);
        filename = gtk_file_chooser_get_filename (chooser);
        GFile *file=g_file_new_for_path(filename);
        GFileInputStream *input=g_file_read(
                file,
                NULL,
                NULL);
        const gsize count =  10000;
        gchar buf[1000];
        g_input_stream_read(G_INPUT_STREAM(input), buf, count, NULL, NULL
        );
        g_print(buf);
        g_input_stream_close(G_INPUT_STREAM(input),NULL,NULL);
        g_free(buf);
        g_object_unref(input);
        g_object_unref(file);
    }
    return 0;
}
```

## Збереження файла із GtkFileChooserDialog

```cpp
#include <gtk/gtk.h>

int main(int argc, char *argv[])
{
    GtkWidget *window;

    gtk_init(&argc,&argv);

    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Hello world");
    gtk_window_set_default_size(GTK_WINDOW(window), 500, 500);
    g_signal_connect(G_OBJECT(window), "destroy", G_CALLBACK(gtk_main_quit), NULL);

    gtk_widget_show_all(window);

    GtkWidget *dialog;
    GtkFileChooserAction action = GTK_FILE_CHOOSER_ACTION_SAVE;
    gint res;

    dialog = gtk_file_chooser_dialog_new ("Save File",
                                          GTK_WINDOW(window),
                                          action,
                                          "Cancel",
                                          GTK_RESPONSE_CANCEL,
                                          "Open",
                                          GTK_RESPONSE_ACCEPT,
                                          NULL);

    res = gtk_dialog_run (GTK_DIALOG (dialog));
    if (res == GTK_RESPONSE_ACCEPT)
    {
        char *filename;
        GtkFileChooser *chooser = GTK_FILE_CHOOSER (dialog);
        filename = gtk_file_chooser_get_filename (chooser);
        GFile *file=g_file_new_for_path(filename);
        GFileOutputStream *out=g_file_replace(
                file,
                NULL,
                FALSE,
                G_FILE_CREATE_NONE,
                NULL,
                NULL);

        gchar *buf=g_strdup_printf("%s", "Hello World!!!");
        g_output_stream_write(G_OUTPUT_STREAM(out),
                              buf,strlen(buf),NULL,NULL);

        g_output_stream_close(G_OUTPUT_STREAM(out),NULL,NULL);
        g_free(buf);
        g_object_unref(out);
        g_object_unref(file);
    }
    return 0;
}
```

# Текстовий редактор

```ui
<?xml version="1.0" encoding="UTF-8"?>
<!-- Generated with glade 3.22.1 -->
<interface>
  <requires lib="gtk+" version="3.20"/>
  <object class="GtkTextTagTable" id="texttagtable1">
    <child type="tag">
      <object class="GtkTextTag" id="justify_center">
        <property name="name">justify_center</property>
        <property name="font">Normal</property>
        <property name="justification">center</property>
      </object>
    </child>
    <child type="tag">
      <object class="GtkTextTag" id="justify_left">
        <property name="name">justify_left</property>
        <property name="font">Normal</property>
      </object>
    </child>
  </object>
  <object class="GtkTextBuffer" id="textbuffer1">
    <property name="tag_table">texttagtable1</property>
  </object>
  <object class="GtkWindow" id="main_window">
    <property name="can_focus">False</property>
    <property name="default_width">600</property>
    <property name="default_height">600</property>
    <child>
      <placeholder/>
    </child>
    <child>
      <object class="GtkBox">
        <property name="visible">True</property>
        <property name="can_focus">False</property>
        <property name="orientation">vertical</property>
        <child>
          <object class="GtkToolbar">
            <property name="visible">True</property>
            <property name="can_focus">False</property>
            <child>
              <object class="GtkToolButton" id="open_file_button">
                <property name="visible">True</property>
                <property name="can_focus">False</property>
                <property name="label" translatable="yes">Open</property>
                <property name="use_underline">True</property>
                <property name="stock_id">gtk-open</property>
                <signal name="clicked" handler="open_file_button_clicked_cb" swapped="no"/>
              </object>
              <packing>
                <property name="expand">False</property>
                <property name="homogeneous">True</property>
              </packing>
            </child>
            <child>
              <object class="GtkToolButton" id="save_button">
                <property name="visible">True</property>
                <property name="can_focus">False</property>
                <property name="label" translatable="yes">Save</property>
                <property name="use_underline">True</property>
                <property name="stock_id">gtk-save-as</property>
                <signal name="clicked" handler="save_button_clicked_cb" swapped="no"/>
              </object>
              <packing>
                <property name="expand">False</property>
                <property name="homogeneous">True</property>
              </packing>
            </child>
            <child>
              <object class="GtkSeparatorToolItem">
                <property name="visible">True</property>
                <property name="can_focus">False</property>
              </object>
              <packing>
                <property name="expand">False</property>
                <property name="homogeneous">True</property>
              </packing>
            </child>
            <child>
              <object class="GtkToolButton" id="left_align_button">
                <property name="visible">True</property>
                <property name="can_focus">False</property>
                <property name="label" translatable="yes">__glade_unnamed_5</property>
                <property name="use_underline">True</property>
                <property name="stock_id">gtk-justify-left</property>
                <signal name="clicked" handler="left_align_button_clicked_cb" swapped="no"/>
              </object>
              <packing>
                <property name="expand">False</property>
                <property name="homogeneous">True</property>
              </packing>
            </child>
            <child>
              <object class="GtkToolButton" id="center_align_button">
                <property name="visible">True</property>
                <property name="can_focus">False</property>
                <property name="label" translatable="yes">__glade_unnamed_5</property>
                <property name="use_underline">True</property>
                <property name="stock_id">gtk-justify-center</property>
                <signal name="clicked" handler="center_align_button_clicked_cb" swapped="no"/>
              </object>
              <packing>
                <property name="expand">False</property>
                <property name="homogeneous">True</property>
              </packing>
            </child>
            <child>
              <object class="GtkToolButton" id="right_align_button">
                <property name="visible">True</property>
                <property name="can_focus">False</property>
                <property name="label" translatable="yes">__glade_unnamed_5</property>
                <property name="use_underline">True</property>
                <property name="stock_id">gtk-justify-right</property>
              </object>
              <packing>
                <property name="expand">False</property>
                <property name="homogeneous">True</property>
              </packing>
            </child>
            <child>
              <object class="GtkToolButton" id="fill_align_button">
                <property name="visible">True</property>
                <property name="can_focus">False</property>
                <property name="label" translatable="yes">__glade_unnamed_5</property>
                <property name="use_underline">True</property>
                <property name="stock_id">gtk-justify-fill</property>
              </object>
              <packing>
                <property name="expand">False</property>
                <property name="homogeneous">True</property>
              </packing>
            </child>
            <child>
              <object class="GtkSeparatorToolItem">
                <property name="visible">True</property>
                <property name="can_focus">False</property>
              </object>
              <packing>
                <property name="expand">False</property>
                <property name="homogeneous">True</property>
              </packing>
            </child>
            <child>
              <object class="GtkToolButton" id="color_chooser_button">
                <property name="visible">True</property>
                <property name="can_focus">False</property>
                <property name="label" translatable="yes">__glade_unnamed_6</property>
                <property name="use_underline">True</property>
                <property name="stock_id">gtk-select-color</property>
                <signal name="clicked" handler="color_chooser_button_clicked_cb" swapped="no"/>
              </object>
              <packing>
                <property name="expand">False</property>
                <property name="homogeneous">True</property>
              </packing>
            </child>
          </object>
          <packing>
            <property name="expand">False</property>
            <property name="fill">True</property>
            <property name="position">0</property>
          </packing>
        </child>
        <child>
          <object class="GtkTextView" id="text_view">
            <property name="visible">True</property>
            <property name="can_focus">True</property>
            <property name="buffer">textbuffer1</property>
          </object>
          <packing>
            <property name="expand">True</property>
            <property name="fill">True</property>
            <property name="position">1</property>
          </packing>
        </child>
      </object>
    </child>
  </object>
</interface>

```

```cpp
#include <gtk/gtk.h>


extern "C" void save_button_clicked_cb(GtkToolButton*, gpointer);
extern "C" void open_file_button_clicked_cb(GtkToolButton*, gpointer);
extern "C" void left_align_button_clicked_cb(GtkToolButton*, gpointer);
extern "C" void center_align_button_clicked_cb(GtkToolButton*, gpointer);
extern "C" void color_chooser_button_clicked_cb(GtkToolButton*, gpointer);

//WARNING: GLOBAL DATA DETECTED
GtkWidget* textView;
GtkWidget* window;


int main (int argc, char *argv[])
{
    GtkBuilder *builder;

    gtk_init(&argc, &argv);

    builder = gtk_builder_new();
    gtk_builder_add_from_file (builder, "ui/text_editor_view.glade", NULL);

    window = GTK_WIDGET(gtk_builder_get_object(builder, "main_window"));
    textView = GTK_WIDGET(gtk_builder_get_object(builder, "text_view"));

    g_signal_connect(window, "destroy",
                     G_CALLBACK(gtk_main_quit), NULL);

    gtk_builder_connect_signals(builder, NULL);

    gtk_widget_show_all(window);

    gtk_main();

    return 0;
}

void save_button_clicked_cb(GtkToolButton* btn, gpointer data){
    GtkWidget *dialog;
    GtkFileChooserAction action = GTK_FILE_CHOOSER_ACTION_SAVE;
    gint res;
    dialog = gtk_file_chooser_dialog_new ("Save File",
                                          GTK_WINDOW(window),
                                          action,
                                          "Cancel",
                                          GTK_RESPONSE_CANCEL,
                                          "Open",
                                          GTK_RESPONSE_ACCEPT,
                                          NULL);

    res = gtk_dialog_run (GTK_DIALOG (dialog));
    if (res == GTK_RESPONSE_ACCEPT)
    {
        char *filename;
        GtkFileChooser *chooser = GTK_FILE_CHOOSER (dialog);
        filename = gtk_file_chooser_get_filename (chooser);

        gtk_widget_set_sensitive (GTK_WIDGET(textView), FALSE);
        GtkTextBuffer* buffer = gtk_text_view_get_buffer (GTK_TEXT_VIEW (textView));
        GtkTextIter start, end;
        gtk_text_buffer_get_start_iter (buffer, &start);
        gtk_text_buffer_get_end_iter (buffer, &end);
        gchar* text = gtk_text_buffer_get_text (buffer, &start, &end, FALSE);
        gtk_text_buffer_set_modified (buffer, FALSE);
        gtk_widget_set_sensitive (GTK_WIDGET(textView), TRUE);

        g_print(text);

        gboolean result;
        if (filename != NULL)
            result = g_file_set_contents (filename, text, -1, NULL);
    }
    gtk_widget_destroy(GTK_WIDGET(dialog));
}

void open_file_button_clicked_cb(GtkToolButton* btn, gpointer data){
    g_print("open button clicked");
}

void left_align_button_clicked_cb(GtkToolButton* btn, gpointer data){
    GtkTextBuffer* buffer = gtk_text_view_get_buffer(GTK_TEXT_VIEW(textView));
    //get selected bounds
    GtkTextIter start, end;
    gboolean isSelected = gtk_text_buffer_get_selection_bounds (buffer,
                                                                &start,
                                                                &end);
    if(isSelected){
        gtk_text_buffer_remove_tag_by_name (buffer,
                                            "justify_center",
        &start,
        &end);
        gtk_text_buffer_apply_tag_by_name (buffer,
                                           "justify_left",
                                           &start,
                                           &end);
    }
}

void center_align_button_clicked_cb(GtkToolButton* btn, gpointer data){
    GtkTextBuffer* buffer = gtk_text_view_get_buffer(GTK_TEXT_VIEW(textView));
    //get selected bounds
    GtkTextIter start, end;
    gboolean isSelected = gtk_text_buffer_get_selection_bounds (buffer,
                                          &start,
                                          &end);
    if(isSelected){
        gtk_text_buffer_remove_tag_by_name (buffer,
                                            "justify_left",
                                            &start,
                                            &end);
        gtk_text_buffer_apply_tag_by_name (buffer,
        "justify_center",
        &start,
        &end);
    }
}

void color_chooser_button_clicked_cb(GtkToolButton* btn, gpointer data){
    GtkTextBuffer* buffer = gtk_text_view_get_buffer(GTK_TEXT_VIEW(textView));
    //get selected bounds
    GtkTextIter start, end;
    gboolean isSelected = gtk_text_buffer_get_selection_bounds (buffer,
                                                                &start,
                                                                &end);
    if(isSelected) {
        GtkWidget* colorChooserDialog = gtk_color_chooser_dialog_new ("Choose font color", GTK_WINDOW(window));
        gtk_dialog_run(GTK_DIALOG(colorChooserDialog));
        GdkRGBA color;
        gtk_color_chooser_get_rgba (GTK_COLOR_CHOOSER(colorChooserDialog), &color);
        gtk_text_buffer_create_tag (buffer, "selected-color", "foreground-rgba", &color);
        gtk_text_buffer_apply_tag_by_name (buffer,
                                           "selected-color",
                                           &start,
                                           &end);
        gtk_widget_destroy (colorChooserDialog);
    }

}
```

# Домашнє завдання

Доробіть кнопки позиціонування і відкриття існуючого файла.