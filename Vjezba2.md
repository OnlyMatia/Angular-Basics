<h2>1. Generirajte servis naziva Data Service. U servisu implementirajte metodu "getRandomQuote()" koja će koristiti API 
(https://api.quotable.io/random) kako bi dohvatila nasumični citat. Implementirajte interface IQuote za oblik podataka citata.</h2>

//Najprije u terminalu generirati servis 

```ng g s service/data```

//zatim napraviti novi folder models i u njemu definirat podatke

                models/IQuote.ts
```typescript
export interface IQuote {
    _id: string,
    content: string,
    author: string,
    tags: [
      string
    ],
    authorSlug: string,
    length: number,
    dateAdded: number,
    dateModified: number
}
```

                data.service.ts

```typescript
export class DataService {

  constructor(private http: HttpClient) { }

  getRandomQuote(): Observable<IQuote> {
    return this.http.get<IQuote>('https://api.quotable.io/random');
  }
}
```

<h2>2. Koristeći servis DataService u AboutComponent prikažite nasumični citat. </h2>

                about.component.ts

```typescript
quote: IQuote[] = []

    constructor(private dataService: DataService){
        this.quote = dataService.getRandomQuote()
    }
```

                about.component.html

```html
<div *ngIf="quote">
    <p>{{quote.author}}</p>
    <p>{{quote.content}}</p>
</div>
```

<h2>3.Napravite formu za kontakt u ContactComponent-u. Forma treba sadržavati polja za ime, email adresu i poruku. Kada se forma pošalje, prikažite unesene podatke u konzoli. (Template-Driven Form ili Reactive Form) </h2>

//naprije treba importat ReactiveFormsModule 

                app.module.ts

```typescript
imports: [
    BrowserModule,
    AppRoutingModule,
    ReactiveFormsModule  <--
  ],
```
//zatim 

                contact.component.html

```html
<form [formGroup]="contactForm" (ngSubmit)="onSubmit()">
  <div>
    <label for="name">Ime:</label>
    <input type="text" id="name" formControlName="name" required>
  </div>

  <div>
    <label for="email">Email adresa:</label>
    <input type="email" id="email" formControlName="email" required>
  </div>

  <div>
    <label for="message">Poruka:</label>
    <textarea id="message" formControlName="message" required></textarea>
  </div>
  <button type="submit">Pošalji</button>
</form>
```

                contact.component.ts

```typescript
export class ContactComponent {
  contactForm: FormGroup;

  constructor(private formBuilder: FormBuilder) { 
    this.contactForm = this.formBuilder.group({
      name: ['', Validators.required],
      email: ['', [Validators.required, Validators.email]],
      message: ['', Validators.required]
    });
  }

  onSubmit() {
    if (this.contactForm.valid) {
      console.log(this.contactForm.value);
    }
  }
}
```

<h2>4. Omogućite korisniku da klikom na dugme na Home stranici bude preusmjeren na About stranicu. </h2>

                home.component.html

```html
<button (click)="goToAbout()">About page</button>
```

                home.component.ts
```typescript
export class HomeComponent {

  constructor(private router: Router) { } // Injektiram Router

  navigateToAbout() {
    this.router.navigate(['/about']); 
  }
}
```
// Ili moze se u html odmah napravit

```html
<button><a routerLink="/about">About page</a></button>
```








