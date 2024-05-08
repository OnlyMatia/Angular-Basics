<h2>1. Kreirati novi Angular projekat na Desktop, ime projekta postaviti kao vaše ImePrezime.</h2>

<p style="color:green;">ng new ImePrezime</p>

<h2>2. Generisati sljedeće komponente: HomeComponent, NavComponent, ProductComponent i NotFoundComponent</h2>

Za page komponente stranice
<p style="color:green;">ng g c pages/imeKomponente</p>

Za radece komponente stranice
<p style="color:green;">ng g c components/nav</p>


<h2>3. NavComponent dodati na AppComponent, kako bi ova komponenta mogla biti stalno vidljiva. Na NavComponent potrebno je dodati navigacijsku traku. Na navigacijskoj traci potrebno da se nalaze sljedeća dva linka: Home i Product. Te nakon kreiranja navigacijske trake potrebno je rutirati dodane stavke na način da se otvaraju HomeComponent ili ProductComponent.</h2>

            app.component.html
```html
<app-nav></app-nav>   <--
<main class="container">
  <router-outlet></router-outlet>
</main>
```

            nav.component.html
```html
<nav>
    <ul>
        <li><strong>KCKF</strong></li>
    </ul>
    <ul>
        <li><a [routerLink]="'home'">Home</a></li>
        <li><a [routerLink]="'product'">Product</a></li>
    </ul>
</nav>
```

            app-routing.module.ts
```typescript
const routes: Routes = [
  {
    path: '',
    redirectTo: 'home',
    pathMatch: 'full'
  },
  {
    path: 'home', 
    component: HomeComponent
  },
  {
    path: 'product',
    component: ProductComponent
  },
  {
    path: '**', - sve druge rute da idu na notfound komponentu
    component: NotFoundComponent
  }
];
```

<h2>4. Kreirati interface klasu za listu proizvoda koju ćemo koristiti kroz cijelu aplikaciju. Lista proizvoda se nalazi na dnu dokumenta, potrebno je dodati sve propertije ove liste unutar interface klase. Te svugdje prilikom korištenja liste incijalizovati tu listu pomoću interface-a koji kreirate.</h2>

napravit folder models ili interfaces i u njemu napraviti interface i njemu navesti sve atribute iz objekta i indentifirati kakvog
su tipa podatci

            IInterfaceName.ts
```typescript
export Interface IInterfaceName {
    id: number,
    title: string,
    date: number,
    ...
}
```
<h2>5. Na HomeComponent potrebno je uraditi sljedeće stavke:</h2>
<ul>
    <li>Kreirati listu proizvoda</li>
    <li>Za svaki proizvod ispisati samo naziv proizvoda iz liste koju kreirate.</li>
</ul>

                home.component.html

```html
<div>
    <article *ngFor="let elem of productList">{{elem.name}}</article>
</div>
```
//Ili se moze stavit u novi paragraf {{elem.name}}

                home.component.ts

```typescript
lista: IImeInterface[] = [
    {
        "id": 1,
        "name": "T-shirt",
        "quantity": 20,
        "price": 25.99,
        "dateAdded": "2022-01-01",
        "status": true,
        "type": "Clothing"
    },
    {
        "id": 2,
        "name": "Jeans",
        "quantity": 15,
        "price": 45.99,
        "dateAdded": "2022-01-02",
        "status": true,
        "type": "Clothing"
    }
]
```

<h2>6. Potrebno je postaviti da se HomeComponent učitava defaultno prilikom pokretanja projekta.</h2>

                app-routing.module.ts
```typescript
const routes: Routes = [
  {
    path: '',
    redirectTo: 'home',
    pathMatch: 'full'
  }
```
<h2>7. Na ProductComponent je potrebno uraditi sljedeće stavke:</h2>
<ol>
<li>Kreirati listu proizvoda</li>
<li>Prikazati listu proizvoda na stranici korištenjem tabele</li>
<li>Cijenu formatirati na način da bude prikazana kao valuta EUR</li>
<li>Cijenu formatirati na način da ukoliko je cijena veća od 100 EUR boju teksta cijene obojiti u crveno, a ukoliko je cijena manja od 100 EUR boju teksta cijene obojiti u zeleno.</li>
<li>Status formatirati na način da ukoliko je status true, da bude ispisano Aktivan, a ukoliko je status false, da bude ispisano Neaktivan</li>
<li>Potrebno je da Aktivan/Neaktivan budu buttoni, tako da ukoliko se klikne na status Aktivan određenog proizvoda da se isti prebaci u Neaktivan i obratno.</li>
<ol>

1. Lista se ubaci u Product.component.ts

2. //

```html
<table>
  <thead>
    <tr>
      <th>Naziv</th>
      <th>Količina</th>
      <th>Cijena</th>
      <th>Status</th>
      <th>Kategorija</th>
    </tr>
  </thead>
  <tbody>
    <tr *ngFor="let product of productList">
      <th>{{product.name}}</th>
      <td>{{product.quantity}}</td>
      <td>{{product.price}}</td>
      <td>{{product.status}}</td>
      <td>{{product.type}}</td>
    </tr>
  </tbody>
</table>
```


3. //potrebno je ubaciti pipe curency

```<td>{{product.price | currency:"EUR"}}</td>```


4. //nadodati if u style.color

```<td [style.color]="product.price < 100 ? 'red' : 'green'"```
```>{{product.price | currency:"EUR"}}</td>```


5. //

```<td>{{product.status ? "Aktivan" : "Neaktivan" }}</td>```


6. //

```<button (click)="product.status = !product.status">{{product.status ? 'Aktivan':'Neaktivan'}}</button>```


<h2>8. Na ProductComponent iznad tabele proizvoda dodati button koji će imati mogućnost da sakrije ili prikaže cijelu tabelu. To znači da ukoliko je tabela prikazana, potrebno je da button ima naziv Sakrij tabelu i klikom na button da se tabela sakrije i obratno.</h2>

                product.component.html

```<button (click)="showTable = !showTable">{{showTable ? "Hide Table": "Show Table"}}</button>```

                product.component.ts

```showTable = true;```




