#include <iostream>
#include <windows.h>
#include <fstream>
#include <cstdlib>
#include <string>
#include <vector>
#include <sstream>

using namespace std;

struct Przyjaciel
{
    int id;
    string imie, nazwisko, numerTelefonu, email, adres;
};
string konwerjsaIntNaString ( int liczba)
{
    ostringstream ss;
    ss << liczba;
    string lancuch = ss.str();
    return lancuch;
}
void sprawdzIstnieniePlikuZewnetrznego ()
{
    char nazwaPlikuKontaktow[ ] = "KsiazkaAdresowa.txt";

    string linia="";
    fstream plik;
    plik.open(nazwaPlikuKontaktow, fstream::in | fstream::out);
    if (!plik)
    {
        cout << "Tworze plik KsiazkaAdresowa.";
        Sleep(1000);
        plik.open(nazwaPlikuKontaktow,  fstream::in | fstream::out | fstream::trunc);
        plik.close();
    }
    else
    {
        plik.close();
    }
}
void dodajPrzyjaciela(vector <Przyjaciel> &przyjaciel)
{
    string imie, nazwisko, numerTelefonu, email, adres;
    Przyjaciel Osoba;

    string liniaZDanymiPrzyjaciela = "";
    Osoba;

    cout << endl << "Zapisywanie nowego znajomego." << endl << endl;
    cout << "Podaj imie: ";
    cin >> imie;
    cout << "Podaj nazwisko: ";
    cin >> nazwisko;
    cout << "Podaj numer telefonu: ";
    cin.sync();
    getline(cin,numerTelefonu);
    cout << "Podaj mail: ";
    cin >> email;
    cout << "Podaj adres: ";
    cin.sync();
    getline(cin,adres);


    if (przyjaciel.empty() == true)
    {
        Osoba.id = 1;
    }
    else
    {
        Osoba.id = przyjaciel.back().id+1;
    }

    Osoba.imie = imie;
    Osoba.nazwisko = nazwisko;
    Osoba.numerTelefonu = numerTelefonu;
    Osoba.email = email;
    Osoba.adres = adres;

    przyjaciel.push_back(Osoba);

    fstream plik;
    plik.open("KsiazkaAdresowa.txt", ios::out);

    if (plik.good() == true)
    {
        for (vector <Przyjaciel>::iterator itr = przyjaciel.begin(); itr != przyjaciel.end(); itr++)
        {
            liniaZDanymiPrzyjaciela += konwerjsaIntNaString(itr -> id) + "|";
            liniaZDanymiPrzyjaciela += itr -> imie + "|";
            liniaZDanymiPrzyjaciela += itr -> nazwisko + "|";
            liniaZDanymiPrzyjaciela += itr -> numerTelefonu + "|";
            liniaZDanymiPrzyjaciela += itr -> email + "|";
            liniaZDanymiPrzyjaciela += itr -> adres + "|";

            plik << liniaZDanymiPrzyjaciela << endl;
            liniaZDanymiPrzyjaciela = "";
        }
        plik.close();

        system("pause");
    }
    else
    {
        cout << "Nie udalo sie otworzyc pliku i zapisac do niego danych." << endl;
        system("pause");
    }
}
void wczytajPrzyjaciolZPliku(vector <Przyjaciel> &przyjaciel)
{
    string linia;
    string dane;
    int rozdzielnoscKreskami;
    int ileZnakowWyjac;
    int poczatek;
    int iloscPrzyjaciol;
    fstream plik;
    Przyjaciel pusty;

    przyjaciel.clear();
    plik.open("KsiazkaAdresowa.txt",ios::in);
    if (plik.good() == true)
    {

        while (getline(plik,linia))
        {

            ileZnakowWyjac = 0;
            poczatek = 0;
            rozdzielnoscKreskami = 0;

            for (int i = 0; i < linia.size(); i++)
            {
                ileZnakowWyjac = i - poczatek;
                if (linia[i] == '|')
                {
                    rozdzielnoscKreskami++;
                    dane = linia.substr (poczatek,ileZnakowWyjac);
                    switch (rozdzielnoscKreskami)
                    {
                    case 1:
                        pusty.id = atoi(dane.c_str());
                        break;
                    case 2:
                        pusty.imie = dane;
                        break;
                    case 3:
                        pusty.nazwisko = dane;
                        break;
                    case 4:
                        pusty.numerTelefonu = dane;
                        break;
                    case 5:
                        pusty.email = dane;
                        break;
                    case 6:
                        pusty.adres = dane;
                        break;
                    }
                   poczatek = poczatek + ileZnakowWyjac + 1;
                }
            }
            przyjaciel.push_back(pusty);
            iloscPrzyjaciol++;
            plik.clear();
        }
        plik.close();
    }
}

void szukajPoImieniu(vector <Przyjaciel> &przyjaciel)
{
    string szukaneImie;
    bool wyszukanoPrzyjaciol = 0;
    cout << endl << "Podaj imie: ";
    cin >> szukaneImie;

    for (vector <Przyjaciel>::iterator itr = przyjaciel.begin(); itr != przyjaciel.end(); itr++)
    {
        if (itr -> imie == szukaneImie)
        {
            wyszukanoPrzyjaciol = 1;
            cout << endl;
            cout << "Numer ID:|" << itr -> id << endl;
            cout << itr -> imie << "|" << itr -> nazwisko << endl;
            cout << "Numer telefonu:|" << itr -> numerTelefonu << endl;
            cout << "E-mail:|" << itr -> email << endl;
            cout << "Adres:|" << itr -> adres << endl;
            cout << endl;
        }
    }

    if(!wyszukanoPrzyjaciol)
    {
        cout << endl << "Nie znaleziono osoby o tym imieniu." << endl << endl;
    }
    system("pause");
}

void szukajPoNazwisku(vector <Przyjaciel> &przyjaciel)
{
    string szukaneNazwisko;
    bool wyszukanoPrzyjaciolZNazwiskiem = 0;
    cout << endl << "Podaj nazwisko: ";
    cin >> szukaneNazwisko;

    for (vector <Przyjaciel>::iterator itr = przyjaciel.begin(); itr != przyjaciel.end(); itr++)
    {
        if (itr -> nazwisko == szukaneNazwisko)
        {
            wyszukanoPrzyjaciolZNazwiskiem = 1;
            cout << endl;
            cout << "Numer ID:|" << itr -> id << endl;
            cout << itr -> imie << "|" << itr -> nazwisko << endl;
            cout << "Numer telefonu:|" << itr -> numerTelefonu << endl;
            cout << "E-mail:|" << itr -> email << endl;
            cout << "Adres:|" << itr -> adres << endl;
            cout << endl;
        }
    }

    if(!wyszukanoPrzyjaciolZNazwiskiem)
    {
        cout << endl << "Nie znaleziono osoby o tym nazwisku." << endl << endl;
    }
    system("pause");
}
void wyswietlCalaKsiazke(vector <Przyjaciel> &przyjaciel)
{
    for (vector <Przyjaciel>::iterator itr = przyjaciel.begin(); itr != przyjaciel.end(); itr++)
    {
        cout << endl;
        cout << itr -> id;
        cout << "|" << itr -> imie << "|" << itr -> nazwisko;
        cout << "|" << itr -> numerTelefonu;
        cout << "|" << itr -> email;
        cout << "|" << itr -> adres<<"|";
        cout << endl;
    }
    system("pause");
}
void usunKontakt (vector <Przyjaciel> &przyjaciel)
{
    int szukanyNumerID;

    cout << "Wyszukanie przyjaciela. Podaj numer ID przyjaciela: ";
    cin >> szukanyNumerID;

    for (vector <Przyjaciel>::iterator itr = przyjaciel.begin(); itr != przyjaciel.end(); itr++)
    {
        if (itr -> id == szukanyNumerID)
        {
            itr = przyjaciel.erase(itr);
            cout << "Kontakt zostal usuniety." << endl << endl;
            system("pause");
            break;
        }

    }
}
void edytujKontakt (vector <Przyjaciel> &przyjaciel)
{
    string imie, nazwisko, numerTelefonu, email, adres;
    char pozycjaMenu;

    int szukanyNumerID;
    bool znalezionyPrzyjaciel = 0;
    int pozycjaZnalezionejOsoby = 0;


    cout << "Wyszukanie przyjaciela. Podaj numer jego ID: ";
    cin >> szukanyNumerID;

    for (vector <Przyjaciel>::iterator itr = przyjaciel.begin(); itr != przyjaciel.end(); itr++)
    {
        if (itr -> id == szukanyNumerID)
        {
            znalezionyPrzyjaciel = 1;
            system("cls");
            cout << "Edycja kontaktu." << endl;
            cout << "Zmien imie: " << itr -> imie << endl;
            cout << "Zmien nazwisko: " << itr -> nazwisko << endl;
            cout << "Zmien Numer telefonu: " << itr -> numerTelefonu << endl;
            cout << "Zmien E-mail: " << itr -> email << endl;
            cout << "Zmien Adres: " << itr -> adres << endl;
            cout << endl;
            cout << "1.Edytuj imie." << endl;
            cout << "2.Edytuj nazwisko." << endl;
            cout << "3.Edytuj numer telefonu." << endl;
            cout << "4.Edytuj adres emailowy." << endl;
            cout << "5.Edytuj adres kontaktowy." << endl;
            cout << "6.Edytuj wszystkie informacje." << endl;
            cout << "7.Zakoncz edycje." << endl<<endl;
            cin>> pozycjaMenu;

            switch(pozycjaMenu)
            {
            case '1':
                cout << "Podaj nowe imie: ";
                cin.sync();
                getline(cin,imie);
                przyjaciel[pozycjaZnalezionejOsoby].imie = imie;
                break;
            case '2':
                cout << "Podaj nowe nazwisko: ";
                cin >>  nazwisko;
                przyjaciel[pozycjaZnalezionejOsoby].nazwisko = nazwisko;
                break;
            case '3':
                cout << "Podaj nowy numer telefonu: ";
                cin >>  nazwisko;
                przyjaciel[pozycjaZnalezionejOsoby].numerTelefonu = numerTelefonu;
                break;
            case '4':
                cout << "Podaj nowy e-mail: ";
                cin >>  nazwisko;
                przyjaciel[pozycjaZnalezionejOsoby].email = email;
                break;
            case '5':
                cout << "Podaj nowy adres kontaktowy: ";
                cin.sync();
                getline(cin,adres);
                przyjaciel[pozycjaZnalezionejOsoby].adres = adres;
                break;
            case '6':
                cout << "Podaj nowy numer telefonu: ";
                cin.sync();
                getline(cin,numerTelefonu);
                cout << "Podaj nowy adres e-mailowy: ";
                cin >>  email;
                cout << "Podaj nowy adres kontaktowy: ";
                cin.sync();
                getline(cin,adres);

                przyjaciel[pozycjaZnalezionejOsoby].numerTelefonu = numerTelefonu;
                przyjaciel[pozycjaZnalezionejOsoby].email = email;
                przyjaciel[pozycjaZnalezionejOsoby].adres = adres;
                break;
            case '7':
                system("pause");
                break;
            }
            cout << "Edycja kontaktu powiodla sie." << endl << endl;

        }
        pozycjaZnalezionejOsoby++;
    }

    if(!znalezionyPrzyjaciel)
    {
        cout << endl << "Nie znaleziono osoby o podanym ID." << endl << endl;
        system("pause");
    }
}
void zapisDanychDoPliku (vector <Przyjaciel> &przyjaciel)
{
    fstream plik;
    string liniaZDanymiPrzyjaciela = "";
    plik.open("KsiazkaAdresowa.txt", ios::out);

    if (plik.good() == true)
    {
        for (vector <Przyjaciel>::iterator itr = przyjaciel.begin(); itr != przyjaciel.end(); itr++)
        {
            liniaZDanymiPrzyjaciela += konwerjsaIntNaString(itr -> id) + "|";
            liniaZDanymiPrzyjaciela += itr -> imie + "|";
            liniaZDanymiPrzyjaciela += itr -> nazwisko + "|";
            liniaZDanymiPrzyjaciela += itr -> numerTelefonu + "|";
            liniaZDanymiPrzyjaciela += itr -> email + "|";
            liniaZDanymiPrzyjaciela += itr -> adres + "|";

            plik << liniaZDanymiPrzyjaciela << endl;
            liniaZDanymiPrzyjaciela = "";
        }
        plik.close();
        cout << "Dane zostaly zapisne." << endl;
        system("pause");
    }
    else
    {
        cout << "Nie udalo sie otworzyc pliku i zapisac do niego danych." << endl;
        system("pause");
    }
}

int main()
{
    vector <Przyjaciel> przyjaciel;

    char wyborFunkcji;


    wczytajPrzyjaciolZPliku(przyjaciel);

   // sprawdzIstnieniePlikuZewnetrznego();
    do
    {
        system("cls");
        cout << "Ksiazka adresowa\n\n";
        cout << "1. Dodaj nowego przyjaciela\n";
        cout << "2. Wyszukaj osobe po imieniu\n";
        cout << "3. Wyszukaj osobe po nazwisku\n";
        cout << "4. Wczytaj ksiazke adresowa\n";
        cout << "5. Usun adresata\n";
        cout << "6. Edytuj adresata\n";
        cout << "9. Wyjdz\n\n";


        cout << "Wybierz: ";
        cin >> wyborFunkcji;

        switch(wyborFunkcji)
        {
        case '1':
            dodajPrzyjaciela(przyjaciel);
            break;
        case '2':
            szukajPoImieniu(przyjaciel);
            break;
        case '3':
            szukajPoNazwisku(przyjaciel);
            break;
        case '4':
            wyswietlCalaKsiazke(przyjaciel);
            break;
        case '5':
            usunKontakt(przyjaciel);
            zapisDanychDoPliku(przyjaciel);
            break;
        case '6':
            edytujKontakt(przyjaciel);
            zapisDanychDoPliku(przyjaciel);
            break;
        case '9':
            exit(0);
            break;
        }
    }
    while(true);
}
