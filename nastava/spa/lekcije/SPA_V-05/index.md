---
id: SPA_V-05
summary: Strukture podataka i algoritmi
status: [draft]
authors: Divna Krpan
categories: web
tags: web
source: 1t84JyjWYt3NGwTfu6k_gUy3K5vVXZQgfg8vZskHzRIQ
duration: 0

---

# SPA: Nasljeđivanje




## Uvod



📌 Nasljeđivanje i nadogradnja klasa u .NET/C# okruženju

### Uvod u nasljeđivanje

Nasljeđivanje je jedan od temeljnih pojmova objektno orijentiranog programiranja (OOP), koji omogućava jednoj klasi nasljeđivanje svojstava i metoda druge klase. Ovo znači da klasa koja nasljeđuje (nazivamo je *podklasa* ili *izvedena klasa*), može koristiti sve javne (**public**) i zaštićene (**protected**) metode, varijable i svojstva klase koju nasljeđuje. Klasa od koje nasljeđuje naziva se **osnovna** ili *superklasa*.

U praksi, nasljeđivanje omogućuje ponovnu upotrebu postojećeg kôda, što ubrzava razvoj i smanjuje potrebu za pisanjem kôda koji se ponavlja. Umjesto pisanja nove klase od nule svaki put kada nam je potrebna slična funkcionalnost, možemo stvoriti novu klasu koja nasljeđuje sve elemente postojeće klase, a zatim dodati specifične funkcionalnosti koje su nam potrebne.

> aside positive
> 
> **Napomena:** Na kolegiju Strukture podataka i algoritmi (SPA), nasljeđivanje koristimo na nešto jednostavniji način, s naglaskom na nadogradnju postojećih klasa i dodavanje funkcionalnosti bez detaljne analize ostalih koncepata OOP-a. 

Primjeri koje koristimo mogu uključivati metode za ispis podataka, pretragu i slične osnovne funkcionalnosti koje olakšavaju rad sa strukturama podataka za naše potrebe, a nisu već uključene u postojeću implementaciju.

> aside negative
> 
> **Napomena**: Na kolegiju Objektno orijentirano programiranje (OOP) detaljnije ćete raditi nasljeđivanje i razmatrati složenije primjere. Tamo ćete vidjeti kako možete stvarati složenije hijerarhije klasa i proširivati gotove strukture s obzirom na specifične zahtjeve. Ovdje ćemo se, međutim, fokusirati na jednostavniju primjenu nasljeđivanja u kontekstu struktura podataka.

U *nadogradnji* postojećih klasa cilj je koristiti većinu gotovih mogućnosti neke klase, poput metoda Add(), Clear() i sličnih, dok se samo dodaju određene funkcionalnosti prema specifičnim potrebama zadatka. Tako se štedi vrijeme i omogućava učinkovitije upravljanje kôdom, bez dodatne složenosti koja bi nastala kod ručnog pisanja svake klase od početka.


## Kako "nadograditi" gotove klase?



Kada želimo proširiti funkcionalnost neke već gotove klase, nasljeđivanje nam omogućuje da to učinimo na jednostavan i efikasan način. Umjesto stvaranje potpuno nove klase koja implementira iste funkcionalnosti kao postojeća klasa, možemo stvoriti novu klasu koja nasljeđuje postojeću, a zatim dodati samo one metode koje su specifične za naše potrebe.

Razmotrimo primjer s klasom **Queue** u .NET/C# okruženju. Klasa Queue predstavlja strukturu podataka koja pohranjuje elemente u obliku reda - prvi uneseni element je prvi koji će biti uklonjen (princip "prvi ušao, prvi izašao" ili FIFO). Ova klasa dolazi s već definiranim metodama, poput:

* **Enqueue()** - dodavanje elemenata na kraj reda i
* **Dequeue()** - uklanjanje elemenata s početka reda,

koje omogućuju osnovne operacije nad redom.

Međutim, nama je potrebno proširiti funkcionalnost klase **Queue** jer želimo ispisati sve element na konzolu. Umjesto pisanja nove klase (kao što smo to radili na prvim vježbama), možemo stvoriti klasu koja nasljeđuje Queue ili čak **Queue&lt;int&gt;** i zatim dodati vlastitu metodu za ispis elemenata.

U ovom slučaju, kreirat ćemo klasu **MojQBrojeva** koja nasljeđuje sve funkcionalnosti generičke klase **Queue&lt;int&gt;**, a zatim ćemo dodati novu metodu pod nazivom **Ispis()**. Metoda Ispis() će iterirati kroz sve elemente reda i ispisati ih na konzolu, čime ćemo dobiti mogućnost prikaza sadržaja reda bez potrebe za dodatnim ručnim pristupom elementima.

Evo kako bi izgledala implementacija takve klase:

`MojQBrojeva.cs`

```cs
using System;
using System.Collections.Generic;

public class MojQBrojeva : Queue<int>
{
    // Dodana metoda za ispis elemenata reda
    public void Ispis()
    {
        foreach (int broj in this)
        {
            Console.WriteLine(broj);
        }
    }
}
```

> aside negative
> 
> **Napomena:** U implementaciji metode za ispis koristimo posebnost gotove klase Queue koja dozvoljava iteriranje po elementima reda pomoću petlje!

To nam je ponekad potrebno, ali nije općenito osobina strukture reda jer bi teoretski trebao biti omogućen samo pristup početnom elementu. U klasama i metodama koje smo radili na početnim vježbama to nije bilo moguće, ali ovdje ne želimo pisati sve iz početka već ćemo koristiti ono što nam nudi gotova klasa.

U glavnom dijelu programa možemo primijeniti novu klasu i metodu na sljedeći način:

`Program.cs`

```cs
using System;

namespace Primjer_MojQ;
class Program
{
    static void Main(string[] args)
    {
        int n = 5;
        
        MojQBrojeva red = new MojQBrojeva();

        for (int i = 0; i < n; i++)
        {
            red.Enqueue(i);
        }

        red.Ispis();
    }
}
```

U gornjem primjeru, u petlji **for** unosimo brojeve od 0 do 4 u red. Klasa **Queue&lt;T&gt;** nema metodu za ispis te koristimo našu "nadograđenu" klasu **MojQBrojeva** koja ispisuje elemente reda na konzolu.

Dakle, umjesto instanciranja gotove klase, instanciramo našu te pozivamo metodu **Enqueue()** koja je naslijeđena i novu metodu **Ispis()** koja postoji samo u našoj novoj klasi. Metoda *Ispis()* neće izbaciti elemente iz reda već ih samo ispisuje na konzolu.

Ispis:

```
0
1
2
3
4
```

Klasa **Queue&lt;T&gt;** ima i metodu **ToArray()** koje pretvara red u običan niz. Pri tome dobijemo kopiju elemenata reda:

`MojQBrojeva.cs`

```cs
public void Ispis2()
{
    // koristimo metodu ToArray() za pretvaranje u niz
    int[] niz = this.ToArray();
    Console.WriteLine("Ispis reda:");
    foreach (int element in niz)
    {
        Console.Write("{0} ", element);
    }
    // prijelaz u novu liniju teksta
    Console.WriteLine();
}
```

U gornjoj metodi smo se dodatno prisjetili formatiranog ispisa. U prethodnoj metodi smo pomoću petlje **foreach** izravno pristupali originalnim elementima.

### **Primjer rješenja bez "nadogradnje"**

Naravno da nije nužno koristiti "nadogradnju", ali je korisno jer je kôd često pregledniji te ga možemo koristiti u više programa. Ovdje ćemo vidjeti rješenje prethodnog primjera bez nadogradnje. Obratite pažnju na primjenu ključne riječi **this** u primjeru s nadogradnjom. Moramo voditi računa o sljedećem:

* Sve metode koje se pišu u klasi **Program** moraju biti **statičke** kao što je to i metoda **Main()**.
* U ovom primjeru, metoda **Ispis3()** nema pristup elementima reda, pa prema tome mora **primiti** parametar koji je tipa **Queue&lt;int&gt;** te koristiti instancu (objekt) kojeg je primila.

`Program.cs`

```cs
using System;
using System.Collections.Generic;

namespace Primjer_MojQ;
class Program
{
    static void Ispis3(Queue<int> r)
    {
        foreach (int element in r)
        {
            Console.WriteLine(element);
        }
    }
    static void Main(string[] args)
    {
        int n = 5;
        
        Queue<int> red = new Queue<int>();

        for (int i = 0; i < n; i++)
        {
            red.Enqueue(i);
        }

        // red.Ispis();
        Ispis3(red);
    }
}
```

> aside negative
> 
> Primijetite:
> 
> * Metoda **Ispis3()** ne priprada klasi **Queue&lt;int&gt;,** prima objekt **red** te ispisuje elemente tog objekta.
> * Metoda **Ispis()** pripada klasi **MojQBrojeva** te ispisuje elemente iz reda referencirajući ih unutar klase pomoću ključne riječi **this**.

Što je bolje koristiti? Ovisi o zadatku kojeg rješavamo i eventualnoj budućoj primjeni postojećeg kôda. Ako u vašem zadatku na ispitu piše "nadogradi postojeću klasu" onda **se neće priznati rješenje** sa statičkom metodom u glavnom dijelu programa!


## 📗 Zadatak: Vezana lista



* Napisati program za upis studenata na prvu godinu i formiranje rang liste.
* Koristiti vezanu listu (može se koristiti vlastita ili LinkedList)

Unose se sljedeći podaci:

* Ime i prezime studenta
* Studijska grupa (MI, INF, ...)
* Broj bodova ostvaren na maturi

Nakon unosa ispravnih podataka, svaki student se sprema na kraj liste. 

Dodatno omogućiti:

* Spremanje unesenih studenata u datoteku
* Učitavanje podataka o studentima iz datoteke

### **Izbornik**

<img src="img\\b8f6b8548d70c7ea.png" alt="img\\b8f6b8548d70c7ea.png"  width="241.50" />

Unos podataka

* unos svih podataka o studentu pomoću konzole
* statička metoda Unos() - ne prima ništa, vraća 1 studenta

Spremi u datoteku

* statička metoda Spremi() - prima vezanu listu, ne vraća ništa
* sprema podatke iz vezane liste u datoteku odvojene s #

Učitaj iz datoteke

* statička metoda Ucitaj() - prima vezanu listu, ne vraća ništa
* učitava podatke odvojene s # iz datoteke u vezanu listu

Kraj

* Program prestaje raditi


## 📗 Zadatak: Nova metoda AddSort()



* Dodajte metodu **AddSort()** koja će dodati upisanog studenta sortirano (prema broju bodova) tako da se automatski formira rang lista (bez korištenja algoritama sortiranja)
* Odabirom opcije Učitaj, podaci se učitavaju iz datoteke u listu uz pomoć nove **AddSort()** metode.

<img src="img\\77c122512a8113c5.png" alt="img\\77c122512a8113c5.png"  width="262.50" />


## 📗 Zadatak: Brisanje



Dodati opciju **Briši sve** u izbornik.

* Odabirom opcije, briše se sadržaj cijele liste.

Dodati opciju **Briši 1**:

* Pitati korisnika da unese redni broj od 1 do n (u skladu s ispisom)
* Ako se unese redni broj koji ne postoji, ispisati poruku "Broj ne postoji"
* Ako postoji redni broj, onda je potrebno izbrisati odabrani element iz vezane liste.


## 📗 Zadatak: Raspored pacijenata za pregled



* Za one koji žele više

Napraviti aplikaciju koja će rasporediti pacijente za pregled.

Upute:

* Popis se nalazi u datoteci popis_pacijenata.csv prema redoslijedu prijava.
* Pacijenti imaju pored imena (odvojeno s ; ) i razinu hitnosti.

Hitnost:

* Označena je brojevima od 1-10, s tim da manji broj znači hitnije.
* Ako su iste razine, onda na red dolazi pacijent koji se prije prijavio.

Napraviti i ispisati raspored počevši od sutra pod uvjetom:

* Ordinacija radi svaki dan od 8-16
* Za svakog pacijenta treba 15 minuta

### **Izgled ispisa**

<img src="img\\8ec4607f73c631b0.png" alt="img\\8ec4607f73c631b0.png"  width="217.11" />

Ili:

<img src="img\\3bc1da140c65fc42.png" alt="img\\3bc1da140c65fc42.png"  width="276.35" />


