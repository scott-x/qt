### qt connect sqlite

1. update .pro file, add config  `QT       += core gui` =>`QT       += core gui sql`

2. use

```cpp
#include "mainwindow.h"
#include <QApplication>
//include header
#include <qdebug.h>
#include <QtSql>
#include <QSqlDatabase>
#include <QSqlError>
#include <QSqlQuery>

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);

    //open database
    QSqlDatabase database;
    database = QSqlDatabase::addDatabase("QSQLITE");
    database.setDatabaseName("MyDataBase.db");
    if (!database.open())
    {
        qDebug() << "Error: Failed to connect database." << database.lastError();
    }
    else
    {
        qDebug() << "Succeed to connect database." ;
    }

    //create table
    QSqlQuery sql_query;
    if(!sql_query.exec("create table student(id int primary key, name text, age int)"))
    {
        qDebug() << "Error: Fail to create table."<< sql_query.lastError();
    }
    else
    {
        qDebug() << "Table created!";
    }

    //insert
    if(!sql_query.exec("INSERT INTO student VALUES(1, \"Wang\", 23)"))
    {
        qDebug() << sql_query.lastError();
    }
    else
    {
        qDebug() << "inserted Wang!";
    }
    if(!sql_query.exec("INSERT INTO student VALUES(2, \"Li\", 23)"))
    {
        qDebug() << sql_query.lastError();
    }
    else
    {
        qDebug() << "inserted Li!";
    }

    //update
    sql_query.exec("update student set name = \"QT\" where id = 1");
    if(!sql_query.exec())
    {
        qDebug() << sql_query.lastError();
    }
    else
    {
        qDebug() << "updated!";
    }

    //query
    sql_query.exec("select * from student");
    if(!sql_query.exec())
    {
        qDebug()<<sql_query.lastError();
    }
    else
    {
        while(sql_query.next())
        {
            int id = sql_query.value(0).toInt();
            QString name = sql_query.value(1).toString();
            int age = sql_query.value(2).toInt();
            qDebug()<<QString("id:%1    name:%2    age:%3").arg(id).arg(name).arg(age);
        }
    }

    //delete
    sql_query.exec("delete from student where id = 1");
    if(!sql_query.exec())
    {
        qDebug()<<sql_query.lastError();
    }
    else
    {
        qDebug()<<"deleted!";
    }

    //dete table
    sql_query.exec("drop table student");
    if(sql_query.exec())
    {
        qDebug() << sql_query.lastError();
    }
    else
    {
        qDebug() << "table cleared";
    }

    //close
    database.close();
    return a.exec();
}

```