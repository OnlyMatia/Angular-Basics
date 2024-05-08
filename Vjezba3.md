<h2>1. Kreirati DataService sa dvije funkcije:</h2>
<ul>
    <li>getBooks koja će vraćati sve knjige</li>
    <li>getBooksById koja će vraćati knjigu pomoću njenog Id-a</li>
</ul>

```cmd 
ng g s service/data
```

            data.service.ts

```typescript
constructor() {}

  getBooks(): IBook[] {
    return this.data
  }

  getBookById(id: number): IBook | undefined {
    return this.data.find((book) => book.id === id)
  }
```

<h2>2. Na HomeComponent potrebno je uraditi sljedeće stavke:</h2>
<ul>
    <li>Prikazati listu knjiga u tabeli sa svim detaljima</li>
    <li>Dodati kolonu Action u kojoj će se nalaziti link natpisa Details koji vodi na BookDetailsComponent</li>
    <li>Iznad tabele dodati dugme "Create" koje će voditi na NewBookComponent</li>
</ul>

a. 
            home.component.ts

```typescript
bookList: IBook[] = [];

    constructor(private dataService: DataService){
        this.bookList = dataService.getBooks()
    }
```

```html
<table>
    <thead>
        <tr>
            <th>Stvari</th>
            <th>koje</th>
            <th>cemo ispisivat</th>
        </tr>
    </thead>
    <tbody *ngFor="let book of bookList">
        <tr>
            <td>{{book.prvastvar}}</td>
            <td>{{book.drugastvar}}</td>
            <td>{{book.trecastvar}}</td>
        </tr>
    </tbody>
</table
```

b. 
                home.component.html
```html
<tr>
    <td>{{book.prvastvar}}</td>
    <td>{{book.drugastvar}}</td>
    <td>{{book.trecastvar}}</td>
    <td><a routerLink="/bookdetails">Details</a></td>
</tr>   

c.
                home.component.html

```html
<button><a routerLink="/new">New book</a></button>
```

<h2>3. Na NewBookComponent potrebno je uraditi sljedeće stavke:</h2>
<ul>
    <li>Koristeći Reactive Forms Module napraviti formu</li>
    <li>Postaviti sva polja kao obavezna</li>
    <li>Na submit forme, formu ispisati u konzoli</li>
</ul>























