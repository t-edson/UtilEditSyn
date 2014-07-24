UtilEditSyn
===========

Librería para implementación de las funcionalidades típicas de un editor de texto usando el componente SynEdit.

Incluye funciones para el manejo de los formatos de salto de línea más comunes (UNIX, DOS, MAC) y maneja diversas codificaciones de texto como UTF8, win-1252, etc.

Permite la implementación Sencilla de las funciones de Apertura, Cierre, Nuevo archivo, etc.

Maneja las verificaciones de existencia de archivo, o archivo modificado, mostrando los
diálogos apropiados, en cada caso.

Puede manejar diversos mensajes de estado del editor, usando una barra de estado.

También permite guardar un histórico de los archivos abiertos recientemente.

Para usar la librería, se debe incluir obligatoriamente un control TSynEdit y opcionalmente los diálogos TOpenDialog y TSaveDialog.


Para su uso, declarar un objeto Editor y crearlo y destruirlo apropiadamente:

```
edit: TObjEditor;

...

procedure TForm1.FormCreate(Sender: TObject);
begin
  edit := TObjEditor.Create;

  //inicia control de paneles
  edit.PanFileSaved := StatusBar1.Panels[0]; //panel para mensaje "Guardado"
  edit.PanCursorPos := StatusBar1.Panels[1];  //panel para la posición del cursor

  //al final se debe asignar al editor
  edit.InitEditor(SynEdit1,'NoName', 'txt');

end;

procedure TForm1.FormDestroy(Sender: TObject);
begin
  edit.Free;
end;
```

El primer parámetro de InitEditor(), es el editor con el que se trabajará. Debe llamarse, después de
configurar los eventos y paneles.

El segundo parámetro de InitEditor(), es el nombre del archivo por defecto que se usará cuando se inicie
el editor o se cree un nuevo archivo.

El tercer parámetro de InitEditor(), es la extensión por defecto del archivo por defecto.

Para que el control del editor sea funcional, se debe interceptarse los eventos para abrir, guardar, etc.

```
procedure TForm1.mnNewClick(Sender: TObject);
begin
  edit.NewFile;
end;

procedure TForm1.mnOpenClick(Sender: TObject);
begin
  OpenDialog1.Filter:='Text files|*.txt|All files|*.*';
  edit.OpenDialog(OpenDialog1);
end;

procedure TForm1.mnSaveClick(Sender: TObject);
begin
  edit.SaveFile;
end;

procedure TForm1.mnSaveAsClick(Sender: TObject);
begin
  edit.SaveAsDialog(SaveDialog1);
end;
```

La idea es pasar el control de las acciones, o eventos de menú al nuevo objeto "TObjEditor" creado, en lugar de manejar al SynEdit.


IMPORTANTE. La librería toma el control de los  eventos OnChange(), OnStatusChange(), OnMouseDown y OnCommandProcessed().

Si se necesita usar el evento OnChange(), se debe usar TObjEditor.OnEditChange() .

Para un ejemplo más completo de uso, ver los proyecto de muestra adjuntos.
