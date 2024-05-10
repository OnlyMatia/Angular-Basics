<h2>1. Kreirati interface klasu za listu filmova koju ćemo koristiti kroz cijelu aplikaciju. Lista filmova se nalazi na dnu dokumenta, potrebno je dodati sve propertije ove liste unutar interface klase. Na svim mjestima prilikom korištenja liste inicijalizirati tu listu pomoću interface-a koji kreirate.</h2>

            models/IFilm.ts
```typescript
export interface IFilm {
    id: number,
    movie: string,
    theater: string,
    ticketsAvailable: number,
    ticketPrice: number,
    showDate: string,
    showTime: number
}
```

<h2>2. Kreirati DataService sa dvije funkcije:</h2>
<ul>
    <li>getMovies koja će vraćati sve filmove</li>
    <li>getMoviesById koja će vraćati film pomoću Id-a</li>
</ul>

```cmd
ng g s service/data
```
            data.service.ts
```typescript
export class DataService {

  movies: IFilm[] = [
    { 
      id: 1,
      movie: "The Matrix",
      theater: "Screen 1",
      ticketsAvailable: 120,
      ticketPrice: 10.50,
      showDate: "May 10, 2024",
      showTime: "10:00 AM"
    },
    { 
      id: 3,
      movie: "Inception",
      theater: "Screen 2",
      ticketsAvailable: 90,
      ticketPrice: 11.00,
      showDate: "May 10, 2024",
      showTime: "01:30 PM"
    },
    { 
      id: 2,
      movie: "Interstellar",
      theater: "Screen 3",
      ticketsAvailable: 100,
      ticketPrice: 9.75,
      showDate: "May 10, 2024",
      showTime: "04:45 PM"
    },
    { 
      id: 5,
      movie: "The Dark Knight",
      theater: "Screen 4",
      ticketsAvailable: 80,
      ticketPrice: 12.00,
      showDate: "May 10, 2024",
      showTime: "08:15 PM"
    },
    { 
      id: 4,
      movie: "Avatar",
      theater: "Screen 5",
      ticketsAvailable: 18,
      ticketPrice: 10.00,
      showDate: "May 10, 2024",
      showTime: "10:30 PM"
    }
  ];
  
  constructor() { }
```

//ubaciti listu i assing it IFilm interface radi TS

            data.service.ts
```typescript
getMovies(): IFilm[]{
    return this.movies
  }

  getMoviesById(id : number): IFilm | undefined{
    return this.movies.find((movie) => movie.id === id)
  }
```

<h2>3. Na HomeComponent potrebno je uraditi sljedeće stavke:</h2>
<ul>
    <li>Prikazati listu filmova u tabeli sa svim detaljima</li>
    <li>Dodati kolonu Action u kojoj će se nalaziti link natpisa Edit koji vodi na EditMovieComponent</li>
    <li>Iznad tabele dodati dugme "Hide" koje će sakrivati/prikazivati tabelu</li>
    <li>Dodati pipe za formatiranje cijene i postaviti cijenu kao euro</li>
    <li>Ako je broj dostupnih karata manji od 20, prikazati poruku "Only x tickets left"</li>
</ul>

a. Najprije injektat data list sa data.service na komponentu.ts
            home.component.ts
```typescript
export class HomeComponent {
  movieList: IFilm[] = [] <-- definirat listu kako ce se zvat

  constructor(private dataService: DataService){
    return this.movieList = dataService.getMovies() <-- pozvat preko dataServica tu funk koja vraca listu
  }
}
```
            home.component.html
```html
<table>
    <thead>
        <tr>
            <th>movie</th>
            <th>showTIME</th>
            ...
        </tr>
    </thead>
    <tbody *ngFor="let movie of movieList">
        <tr>
            <td>{{movie.movie}}</td>
            <td>{{movie.showTime}}</td>
            ...
        </tr>
    </tbody>
</table>
```

b. 
          home.component.html
```html
<tr>
            <th>movie</th>
            <th>showTIME</th>
            <th>Action</th>
            ...
        </tr>
    </thead>
    <tbody *ngFor="let movie of movieList">
        <tr>
            <td>{{movie.movie}}</td>
            <td>{{movie.showTime}}</td>
            <td><a [routerLink]="['/edit', movie.id]">details</a><td>  <---
            ...
        </tr>
    </tbody>
```

c.
          home.component.html
```html
<button (click)="showTable = !showTable">{{showTable ? "Hide Table": "Show Table"}}</button>
<table *ngIf="showTable">
```

          home.component.ts
```typescript
showTable: boolean = true;
```
d. 

          home.component.html
```html
<td>{{movie.ticketPrice | currency:'EUR'}}</td>
```

e.
          home.component.html
```html
<td>{{movie.ticketsAvailable < 20 ? 'Only ' + movie.ticketsAvailable + ' tickets left' : movie.ticketsAvailable}}</td>
```


<h2>4. Na EditMovieComponent potrebno je uraditi sljedeće stavke:</h2>
<ul>
  <li>Prikazati detalje odabranog filma unutar forme (Template Driven ili Reactive Form)</li>
  <li>Omogućiti korisniku da izmijeni podatke o filmu i na save prikazati novi film u konzoli</li>
</ul>

a.// template driven form
                app.modules.ts
```typescript
rowserModule,
    AppRoutingModule,
    FormsModule
```
            editmovie.component.html
```html
<form #movieForm="ngForm" (ngSubmit)="onSubmit(movieForm.value)">
    <div>
        <label for="movie">Movie:</label>
        <input type="text" id="movie" name="movie" [(ngModel)]="movie.movie" required>
    </div>
    <div>
        <label for="theater">Theater:</label>
        <input type="text" id="theater" name="theater" [(ngModel)]="movie.theater" required>
    </div>
    <div>
        <label for="ticketsAvailable">Tickets Available:</label>
        <input type="number" id="ticketsAvailable" name="ticketsAvailable" [(ngModel)]="movie.ticketsAvailable" >
    </div>
    <div>
        <label for="ticketPrice">Ticket Price:</label>
        <input type="number" id="ticketPrice" name="ticketPrice" [(ngModel)]="movie.ticketPrice" >
    </div>
    <div>
        <label for="showDate">Show Date:</label>
        <input type="text" id="showDate" name="showDate" [(ngModel)]="movie.showDate" required>
    </div>
    <div>
        <label for="showTime">Show Time:</label>
        <input type="text" id="showTime" name="showTime" [(ngModel)]="movie.showTime" required>
    </div>
    <button type="submit" >Submit</button>
</form>
```

              editmovie.component.ts
```typescript
export class EditMovieComponent {
  
  movie: IMovie = {
    id: 0,
    movie: '',
    theater: '',
    ticketsAvailable: 0,
    ticketPrice: 0,
    showDate: '',
    showTime: '',
  };

  constructor(private dataService: DataService, private route: ActivatedRoute) {
    let id = this.route.snapshot.params['id'] as number;
    this.movie = this.dataService.getMovieById(id);
  }

  onSubmit(formData: IMovie) {
    console.log(formData);
  }
}
```

<h2>5. Dodati validaciju na Formi u EditMovieComponent:</h2>
<ul>
```html
<button type="submit" [disabled]="movieForm.invalid">Submit</button>
```
<li>Polje "Tickets Available" ne smije biti prazno i mora biti broj veći od 0</li>

```html
<div>
  <label for="ticketsAvailable">Tickets Available:</label>
  <input type="number" id="ticketsAvailable" name="ticketsAvailable" [(ngModel)]="movie.ticketsAvailable" required min="1"> <---
</div>
```

<li>Polje "Ticket Price" ne smije biti prazno i mora biti broj veći od 0</li>
```html
<div>
  <label for="ticketPrice">Ticket Price:</label>
  <input type="number" id="ticketPrice" name="ticketPrice" [(ngModel)]="movie.ticketPrice" required min="1"> <---
</div>
```