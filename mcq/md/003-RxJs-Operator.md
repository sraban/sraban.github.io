RxJS operators are powerful tools for handling asynchronous events and streams of data in a reactive programming style. Here are some of the most commonly used RxJS operators, along with examples to illustrate their usage:

### 1. `map`

Transforms the items emitted by an observable by applying a function to each item.

**Example:**
```typescript
import { of } from 'rxjs';
import { map } from 'rxjs/operators';

of(1, 2, 3)
  .pipe(map(x => x * 2))
  .subscribe(value => console.log(value));
// Output: 2, 4, 6
```

### 2. `filter`

Filters the items emitted by an observable by only emitting those that satisfy a specified predicate.

**Example:**
```typescript
import { of } from 'rxjs';
import { filter } from 'rxjs/operators';

of(1, 2, 3, 4, 5)
  .pipe(filter(x => x % 2 === 0))
  .subscribe(value => console.log(value));
// Output: 2, 4
```

### 3. `mergeMap` (or `flatMap`)

Projects each source value to an observable which is merged in the output observable.

**Example:**
```typescript
import { of } from 'rxjs';
import { mergeMap } from 'rxjs/operators';

of(1, 2, 3)
  .pipe(mergeMap(x => of(x * 2)))
  .subscribe(value => console.log(value));
// Output: 2, 4, 6
```

### 4. `switchMap`

Projects each source value to an observable which is merged in the output observable, emitting values only from the most recently projected observable.

**Example:**
```typescript
import { of, interval } from 'rxjs';
import { switchMap, take } from 'rxjs/operators';

of(1, 2, 3)
  .pipe(switchMap(x => interval(1000).pipe(take(3))))
  .subscribe(value => console.log(value));
// Output: 0, 1, 2 (three times, once for each value in the source observable)
```

### 5. `concatMap`

Projects each source value to an observable which is concatenated in the output observable.

**Example:**
```typescript
import { of } from 'rxjs';
import { concatMap, delay } from 'rxjs/operators';

of(1, 2, 3)
  .pipe(concatMap(x => of(x).pipe(delay(1000))))
  .subscribe(value => console.log(value));
// Output: 1 (after 1s), 2 (after 2s), 3 (after 3s)
```

### 6. `catchError`

Catches errors on the observable to be handled by returning a new observable or throwing an error.

**Example:**
```typescript
import { of, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';

throwError('This is an error!')
  .pipe(catchError(error => of(`Caught: ${error}`)))
  .subscribe(value => console.log(value));
// Output: Caught: This is an error!
```

### 7. `debounceTime`

Emits a value from the source observable only after a particular time span has passed without another source emission.

**Example:**
```typescript
import { of } from 'rxjs';
import { debounceTime } from 'rxjs/operators';

of(1, 2, 3)
  .pipe(debounceTime(1000))
  .subscribe(value => console.log(value));
// Output: 3 (only the last value is emitted after 1s of silence)
```

### 8. `distinctUntilChanged`

Suppresses duplicate items emitted by an observable.

**Example:**
```typescript
import { of } from 'rxjs';
import { distinctUntilChanged } from 'rxjs/operators';

of(1, 1, 2, 2, 3, 3)
  .pipe(distinctUntilChanged())
  .subscribe(value => console.log(value));
// Output: 1, 2, 3
```

### 9. `combineLatest`

Combines multiple observables to create an observable whose values are calculated from the latest values of each of its input observables.

**Example:**
```typescript
import { combineLatest, of } from 'rxjs';

const observable1 = of(1, 2, 3);
const observable2 = of('a', 'b', 'c');

combineLatest([observable1, observable2]).subscribe(value => console.log(value));
// Output: [3, 'c']
```

### 10. `forkJoin`

Waits for all provided observables to complete and then emits a single array containing their last emitted values.

**Example:**
```typescript
import { of, forkJoin } from 'rxjs';
import { delay } from 'rxjs/operators';

const observable1 = of(1).pipe(delay(1000));
const observable2 = of('a').pipe(delay(2000));

forkJoin([observable1, observable2]).subscribe(value => console.log(value));
// Output: [1, 'a'] (after 2s)
```


### 11. `distinctUntilChanged`
Suppresses duplicate items emitted by the source observable.

```typescript
import { of } from 'rxjs';
import { distinctUntilChanged } from 'rxjs/operators';

of(1, 1, 2, 2, 3, 3)
  .pipe(distinctUntilChanged())
  .subscribe(console.log); // Output: 1, 2, 3
```

### 12. `take`
Emits only the first `n` values emitted by the source observable.

```typescript
import { interval } from 'rxjs';
import { take } from 'rxjs/operators';

interval(1000)
  .pipe(take(3))
  .subscribe(console.log); // Output: 0, 1, 2 (one per second)
```


### 11. `startWith`
Emit a specified sequence of items before beginning to emit the items from the source observable.

```typescript
import { of } from 'rxjs';
import { startWith } from 'rxjs/operators';

of(1, 2, 3)
  .pipe(startWith(0))
  .subscribe(console.log); // Output: 0, 1, 2, 3
```

### 13. `tap` (formerly `do`)
Performs side effects for notifications from the source observable.

```typescript
import { of } from 'rxjs';
import { tap } from 'rxjs/operators';

of(1, 2, 3)
  .pipe(tap(value => console.log(`Side effect: ${value}`)))
  .subscribe(console.log); // Output: "Side effect: 1", 1, "Side effect: 2", 2, "Side effect: 3", 3
```


These examples cover some of the most commonly used RxJS operators. Each operator has its own specific use case and can be combined with others to handle complex asynchronous scenarios in a clean and readable way.