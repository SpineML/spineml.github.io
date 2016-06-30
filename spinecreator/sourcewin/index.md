# Building on Windows

SpineCreator is built using the Qt toolkit (http://qt.io), a cross-platform library
of C++ code for building desktop apps. This means that SpineCreator can be compiled 
on Windows just as well as on Mac or Linux. However, we only support Mac and Linux
at present as "first class citizens", due to a lack of developer resource.

SpineML_2_BRAHMS in particular would require some work to function on Windows, as
it includes bash scripts. Microsoft have recently added bash to Windows 10, so
this may now be less arduous that it once was, though at present this is not
available in non-developer installations of Windows.

For now, if you would like to use SpineCreator on Windows as an editor, then please
go ahead and install Qt, graphviz and python, then compile SpineCreator in QtCreator.
