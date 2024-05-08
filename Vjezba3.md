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
</table>
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

            app.modules.ts
```typescript
    imports: [
    BrowserModule,
    AppRoutingModule,
    ReactiveFormsModule  <---
  ],
```

            newbook.component.html
```html
<form [formGroup]="bookForm" (ngSubmit)="onSubmit()">
    <div>
        <label for="title">Title:</label>
        <input type="text" id="title" name="title" formControlName="title" required>
    </div>

    <div>
        <label for="author">Author:</label>
        <input type="text" id="author" name="author" formControlName="author" required>
    </div>

    <div>
        <label for="genre">Genre:</label>
        <input type="text" id="genre" name="genre" formControlName="genre" required>
    </div>

    <div>
        <label for="publication_year">Publication Year:</label>
        <input type="number" id="publication_year" name="publication_year" formControlName="publication_year" required>
    </div>

    <button type="submit">Add Book</button>
</form>
```
            newbook.component.ts

```typescript
export class NewBookComponent {
  bookForm: FormGroup;

  constructor(private formBuilder: FormBuilder) {
    this.bookForm = this.formBuilder.group({
      title: ['', Validators.required],
      author: ['', Validators.required],
      genre: ['', Validators.required],
      publication_year: ['', Validators.required],
    });
  }

  onSubmit() {
    if (this.bookForm.valid) {
      const newBook: IBook = {
        id: Math.floor(Math.random() * 10) + 1,
        title: this.bookForm.value.title,
        author: this.bookForm.value.author,
        genre: this.bookForm.value.genre,
        publication_year: this.bookForm.value.publication_year,
      };

      console.log(newBook);
    }
  }
}
```


<h2>4. Na BookDetailsComponent potrebno je uraditi sljedeće stavke:</h2>
<ul>
    <li>Prikazati detalje knjige</li>
    <li>Dodaj dugme koje će prikazivati i sakrivati detalje knjige</li>
    <li>Dodaj dugme koje će vraćati korisnika nazad na HomeComponent</li>
</ul>

//najprije treba modifirati rute da bi dosli do bookdetails

                app-routing.module.ts

```typescript
 {
    path: 'details/:id',
    component: BookDetailsComponent
  },
```

            home.component.html
```html
<td>                              nadodat u array book.id
    <a [routerLink]="['/details', book.id]">Details</a>
</td>
```

            bookdetails.component.ts
```typescript
export class BookDetailsComponent {
  id: string = '';
  book!: IBook

  constructor(private route: ActivatedRoute, private dataService: DataService) {}

  ngOnInit() {
    this.route.params.subscribe((params) => {
      this.id = params['id'];
      this.book = this.dataService.getBookById(Number(this.id)) as IBook;
    });
  }
}
```

            bookdetails.component.html
```html
<button (click)="!show">Show</button>
<table *ngIf="show">
    <thead>
```

            bookdetails.component.ts
```typescript
show=true
```

            bookdetails.component.html
```html
    </tbody>
</table>
<button><a routerLink="/">Home</a></button>
```


<h2>5. Napraviti CenturyPipe koji će na osnovu godine publikacije knjige vraćati u kojem je stoljeću knjiga napisana</h2>

```cmd
ng g p pipes/century
```

                century.pipe.ts
```typescript
export class CenturyPipe implements PipeTransform {

  transform(value: number): string {
    const century = Math.ceil(value / 100);
    return `${century}th century`;
  }
}
```

<h2>6. U HomeComponent tabeli dodati novu kolonu Century i koristeći CenturyPipe prikazati stoljeće u kojem je napisana knjiga</h2>

            home.component.html
```html
...
    <td>{{book.publication_year | century }}</td>
</tr>
```

<h2>7. -Oznaciti godinu izdavanja knjige crvenom bojom ukoliko je godina izdavanja manja od 1900
-Oznaciti godinu izdavanja knjige zelenom bojom ukoliko je godina izdavanja veca od 1900</h2>


                home.component.html
```html
<td>{{book.genre}}</td>

<td [style.color]="book.publication_year < 1900 ? 'red' : 'green'">{{book.publication_year}}</td>

<td>{{book.publication_year | century }}</td>
```

<h2>8. Rutirati sve nepostojeće putanje na HomeComponent</h2>

                app-routing-module.ts
```typescript
{
    path: '**',
    component: HomeComponent
}
```




