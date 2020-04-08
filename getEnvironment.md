# qt get system environment 

### method 1
```cpp
QString path = QProcessEnvironment::systemEnvironment().value("HOME");
```

### method 2
```cpp
QStringList environment = QProcess::systemEnvironment(); 
QString str; foreach(str,environment) 
{
     if (str.startsWith("PATH="))
     {
         ui.textEdit->append(str);
         break;
     }
} 
```