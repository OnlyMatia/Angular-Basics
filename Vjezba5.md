<h2>1. NavComponent dodati na AppComponent, kako bi ova komponenta mogla biti stalno vidljiva. Na NavComponent potrebno je dodati navigacijsku traku. Na navigacijskoj traci potrebno da se nalaze sljedeća dva linka: Home i Urgent. Te nakon kreiranja navigacijske trake potrebno je rutirati dodane stavke na način da se otvaraju HomeComponent ili UrgentComponent. </h2>


            app-routing.module.ts
```typescript
const routes: Routes = [
  {
    path: 'home',
    component: HomeComponent,
  },
  {
    path: 'urgent',
    component: UrgentComponent,
  },
  {
    path: '',
    redirectTo: 'home',
    pathMatch: 'full',
  },
  {
    path: '**',
    component: NotFoundComponent
  },
];
```

            nav.component.html
```html
<nav>
        <ul>
            <li>logo</li>
        </ul>
        <ul>
            <li>
                <a routerLink="/home">Home</a>
            </li>
            <li>
                <a routerLink="/urgent">Urgent</a>
            </li>
        </ul>
    </nav>
```

<h2>2.  Kreirati interface klasu za listu zadataka koju ćemo koristiti kroz cijelu aplikaciju. Lista zadataka se nalazi na dnu dokumenta, potrebno je dodati sve propertije ove liste unutar interface klase. Te svugdje prilikom korištenja liste incijalizovati tu listu pomoću interface-a koji kreirate.</h2>


            models/ITodo.ts
```typescript
export interface ITodo {
    id: number,
    task: string,
    description: string,
    dueDate: string,
    priority: string,
    completed: boolean
}
```

<h2>3. Na HomeComponent potrebno je uraditi sljedeće stavke:</h2>
<ul>
    <li>Kreirati listu zadataka</li>
    <li>Prikazati listu zadataka na stranici korištenjem tabele</li>
    <li>Tabelu formatirati na način ukoliko je priority zadatka 'High' boju teksta priority polja obojiti u crveno, a ukoliko je priority 'Low' boju teksta obojiti u zeleno.</li>
    <li>Status formatirati na način da ukoliko je completed true, da bude ispisano "Completed", a ukoliko je completed false, da bude ispisano "To Do"</li>
    <li>Potrebno je dodati Action kolonu i unutar nje button , tako da ukoliko se klikne na status Completed određenog zadatka da se isti prebaci u To Do i obratno.</li>
</ul>

            home.component.html
```html
<table>
    <thead>
        <tr>
            <th>Task</th>
            <th>Desc</th>
            <th>Due Date</th>
            <th>Priority</th>
            <th>Completed</th>
            <th>Action</th>
        </tr>
    </thead>
    <tbody>
        <tr *ngFor="let todo of todoList">
            <th>{{todo.task}}</th>
            <td>{{todo.description}}</td>
            <td>{{todo.dueDate}}</td>
            <td [style.color]="todo.priority === 'High' ? 'red' 
        : (todo.priority === 'Low' && 'green')">{{todo.priority}}</td>
            <td>{{todo.completed ? 'Complete':'To Do'}}</td>
            <td>
                <button (click)="todo.completed = !todo.completed">{{"Action"}}</button>
            </td>
        </tr>
    </tbody>
</table>
```
            home.component.ts
```typescript
todoList: ITodo[] = [
    {
      "id": 1,
      "task": "Buy groceries",
      "description": "Milk, eggs, bread, vegetables",
      "dueDate": "2024-05-15",
      "priority": "High",
      "completed": false
    },
    {
      "id": 2,
      "task": "Finish homework",
      "description": "Complete math assignment",
      "dueDate": "2024-05-13",
      "priority": "Medium",
      "completed": false
    },
    {
      "id": 3,
      "task": "Call mom",
      "description": "Wish her happy birthday",
      "dueDate": "2024-05-12",
      "priority": "Low",
      "completed": false
    },
    {
      "id": 4,
      "task": "Go for a run",
      "description": "Run for 30 minutes",
      "dueDate": "2024-05-12",
      "priority": "Medium",
      "completed": false
    },
    {
      "id": 5,
      "task": "Clean the house",
      "description": "Vacuum, mop floors, clean windows",
      "dueDate": "2024-05-12",
      "priority": "High",
      "completed": false
    }
  ]

```

<h2>4. Na UrgentComponent je potrebno uraditi sljedeće stavke:</h2>
<ul>
    <li>Kreirati listu zadataka samo onih koji su 'High' priority</li>
    <li>Za svaki zadatak ispisati samo naziv zadatka i description.</li>
</ul>

            urgent.component.html
```html
<table>
    <thead>
        <tr>
            <th>naziv</th>
            <th>opis</th>
        </tr>
    </thead>
    <tbody *ngFor="let todo of todoList">
        <tr *ngIf="todo.priority === 'High'">
            <th>{{todo.task}}</th>
            <td>{{todo.description}}</td>
        </tr>
    </tbody>
</table>
```

            urgent.component.ts
```typescript
todoList: ITodo[] = [
    {
      "id": 1,
      "task": "Buy groceries",
      "description": "Milk, eggs, bread, vegetables",
      "dueDate": "2024-05-15",
      "priority": "High",
      "completed": false
    },
    {
      "id": 2,
      "task": "Finish homework",
      "description": "Complete math assignment",
      "dueDate": "2024-05-13",
      "priority": "Medium",
      "completed": false
    },
    {
      "id": 3,
      "task": "Call mom",
      "description": "Wish her happy birthday",
      "dueDate": "2024-05-12",
      "priority": "Low",
      "completed": false
    },
    {
      "id": 4,
      "task": "Go for a run",
      "description": "Run for 30 minutes",
      "dueDate": "2024-05-12",
      "priority": "Medium",
      "completed": false
    },
    {
      "id": 5,
      "task": "Clean the house",
      "description": "Vacuum, mop floors, clean windows",
      "dueDate": "2024-05-12",
      "priority": "High",
      "completed": false
    }
  ]
```


<h2>5. Na UrgentComponent iznad tabele zadataka dodati button koji će imati mogućnost da sakrije ili prikaže cijelu tabelu. To znači da ukoliko je tabela prikazana, potrebno je da button ima naziv Sakrij tabelu i klikom na button da se tabela sakrije i obratno.</h2>

                urgent.component.html
```html
<button (click)="showTable = !showTable">Click</button>
<table *ngIf="showTable">
```
            urgent.component.ts

```typescript
showTable = true
```




















































