# SQLiteBasics

SQLite, dünyada en çok dağıtılan ve tavsiye edilen kaynak kodları halka açık, tamamen C/C++ programlama dilleriyle geliştirilmiş sunucu yazılımı ve yapılandırma gereksinimi olmayan, işlemsel ve ilişkisel bir SQL veritabanı motorudur.

SQLite, onlarca programlama dili içerisinde rahatlıkla kullanılabilir. Bunlardan bazıları ASP, BASIC, C#, C, C++, Common Lisp, Curl, D, Delphi, Haskell, Java, Lua, newLisp, Objective-C, OCaml, Perl, PHP, Python, R, REBOL, Ruby, Scheme, Smaltalk, Tcl ve Visual Basic'tir.

Android Studio ortamında yazılmış bir kod örneği bulunmaktadır.
Hatta kod yazmak isterseniz https://sqliteonline.com/ sitesini ziyaret ederek istediğiniz çalışmayı yapabilirsiniz.
```ruby
package com.beyzaakkuzu.sqlitelearning;

import androidx.appcompat.app.AppCompatActivity;

import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.os.Bundle;

import java.sql.SQLOutput;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //try and catch içerisinde bir veritabanı
        //try and catch: de hata olması durumunda hata aldığımızda ne olduğunu göreceğiz.
        try {
            SQLiteDatabase database = this.openOrCreateDatabase("Studens", MODE_PRIVATE,null);//veritabanı oluşturduk.(excel gibi düşünülebilir)
            // veritabanı SQLiteDatabase diye oluşturuyoruz.
            //içinde bulunduğumuz main activity den oluşturuyoruz.
            //Students veri tabanı oluşturuyoruz.
            database.execSQL("CREATE TABLE IF NOT EXISTS studens (name VARCHAR ,age INT)");
            // tablo oluşturuyoruz.
            //metin gibi yazılır CREATE TABLE IF NOT EXISTS : eğer daha önceden oluşturulmadıysa tablo oluştur.(name ve age =row)
            //column=data
            database.execSQL("INSERT INTO studens (name, age) VALUES ('BEYZA',20)");
            database.execSQL("INSERT INTO studens (name, age) VALUES ('CAN',23)");
            // veritabanına insert into ile ekleme yapıyoruz (öğrenci içeriisndeki name ve age değerlerine veri ekliyoruz.
            // verileri okuyup bize bildiriyor. (cursor= imleç)
            Cursor cursor = database.rawQuery("SELECT * FROM studens", null);
            // veritabanından sorgulama yapmak. Sorgu yazarak nereler yapılabilir göreceğiz."SELECT * FROM studens": studens içerisindeki her şeyi seç.
            // selectionArg= filtreleme
            int nameIn= cursor.getColumnIndex("name"); //getColumnIndex: integer olarak name'in kaçıncı sütunda aolduğunu verir.
            int ageIn= cursor.getColumnIndex("age");

            while(cursor.moveToNext()){
                System.out.println("Name: "+ cursor.getString(nameIn));
                System.out.println("Age:"+ cursor.getInt(ageIn));
            }
            cursor.close();
        }catch (Exception error){
            error.printStackTrace();
        }

    }
}
```
Yukarıdaki kod içerisinde bir veritabanı nasıl tanımlanır, nasıl oluşturulur, oluşturduğumuz tablo içerisine veri eklemenin nasıl olduğunu gördük.
Şimdi eklediğimiz verilerden nereleri seçmek istediğimizi nasıl seçiyoruz görelim:
Bu kodda yapmak istediğimiz öğrencilerin hepsini seç  where age>22 ancak yaşı 22 den büyük olanları göster. 
```ruby
Cursor cursor= database.rawQuery("SELECT * FROM Studens WHERE age>22", null);
```

Eklediğimiz veriler içerisinde güncelleme silme işlemlerinin nasıl yapıldığına bir göz atalım:
```ruby
database.execSQL("UPDATE studens SET age=24 WHERE name='SUZAN'"); //GÜNCELLEME (kısaca kodu açıklarsak : ismi SUZAN olan öğrencinin yaşını 24 olarak değiştir.)
database.execSQL("DELETE FROM studens WHERE name='SUZAN'"); //SİLME (kısaca: ismi SUZAN olan öğrenciyi bul ve onu veritabanından sil)
```

Örneğin çok fazla veriye sahipseniz ve kolay yoldan bir isme veya soyisime ulaşmak için (hatırlamıyor da olabilirsiniz) LIKE '%s'(sonu s ile biten) veya LIKE 'K%' (K ile başlayan) gibi ifadeler ile bulabilirsiniz.
```ruby
Cursor cursor= database.rawQuery("SELECT * FROM Studens WHERE name LIKE '%n'", null);
```
