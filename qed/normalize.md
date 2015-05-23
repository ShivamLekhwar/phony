## Normalize

Normalizes a number, such that only numbers remain.
Use before saving a normalized form into a database.

This will often raise an error if you try normalizing a non E164-izable number (a number that does not contain enough information to be normalized into an E164 conform number). Use Phony.plausible? for checking if it can be normalized first.

Example:

```ruby
  # No. Wrong. Nada.
  Phony.normalize('364 35 33')
```

Always use it with a country code, as Phony needs it to know what country to normalize for:

```ruby
  Phony.normalize('41443643533').assert            == '41443643533'
  Phony.normalize('+41 44 364 35 33').assert       == '41443643533'
  Phony.normalize('+41 44 364 35 33').assert       == '41443643533'
  Phony.normalize('+41 800 11 22 33').assert       == '41800112233'
  Phony.normalize('John: +41 44 364 35 33').assert == '41443643533'
  Phony.normalize('1 (703) 451-5115').assert       == '17034515115'
  Phony.normalize('1-888-407-4747').assert         == '18884074747'
  Phony.normalize('1.906.387.1698').assert         == '19063871698'
  Phony.normalize('+41 (044) 364 35 33').assert    == '41443643533'
```

### Example countries

#### NANP

Handles a cc.

```ruby
  Phony.normalize('+1 724 999 9999').assert == '17249999999'
```

Handles a cc with cc option.

```ruby
  Phony.normalize('+1 724 999 9999', cc: '1').assert == '17249999999'
```

Handles missing cc correctly.
    
```ruby
  Phony.normalize('(310) 555-2121', cc: '1').assert == '13105552121'
```
    
Normalizes one of these crazy numbers.

```ruby
  Phony.normalize('1 (703) 451-5115').assert == '17034515115'
```

Normalizes another one of these crazy numbers.

```ruby
  Phony.normalize('1-888-407-4747').assert == '18884074747'
```

#### Belarus

```ruby
  Phony.normalize('375 152450911').assert == '375152450911'
```

#### Cambodia

Handles numbers with an extra 0.

```ruby
  Phony.normalize('+855012239134').assert == "85512239134"
```

#### Germany

Handles a number with extra (0).

```ruby
  Phony.normalize('+49 (0) 209 22 33 44 55').assert == '4920922334455'
```

Handles a number with extra 0.

```ruby
  Phony.normalize('+49 0 209 22 33 44 55').assert == '4920922334455'
```

#### Hungary

```ruby
  Phony.normalize('36 0630245506').assert == '360630245506'
```

#### Lithuania

```ruby
Phony.normalize('370 8 5 1234567').assert == '370851234567'
```

#### Netherlands

Handles the Dutch number (without US cc) correctly.

```ruby
  Phony.normalize('310 5552121').assert == '315552121'
```

#### Russia

```ruby
  Phony.normalize('7 8 342 1234567').assert == '783421234567'
```
    
### Special Cases 

Normalizes a too short number.

```ruby
  Phony.normalize('+972').assert == '972'
```

Normalizes an already normalized number.

```ruby
  Phony.normalize('41443643533').assert == '41443643533'
```
    
Normalizes a formatted number.

```ruby
  Phony.normalize('+41 44 364 35 33').assert == '41443643533'
```

Normalizes a 00 number.

```ruby
  Phony.normalize('0041 44 364 35 33').assert == '41443643533'
```

Normalizes a service number.

```ruby
  Phony.normalize('+41 800 11 22 33').assert == '41800112233'
```

Removes characters.

```ruby
  Phony.normalize('John: +41 44 364 35 33').assert == '41443643533'
```
    
Normalizes a number with colons.

```ruby
  Phony.normalize('1.906.387.1698').assert == '19063871698'
```

Normalizes an erroneous zero.

```ruby
  Phony.normalize('+410443643533').assert == '41443643533'
```

Does not normalize a correct 0 away.

```ruby
  Phony.normalize('+390909709511').assert == '390909709511'
```

Handles completely crazy 'numbers'.

```ruby
  Phony.normalize('Hello, I am Cora, the 41th parrot, and 44 is my 364 times 35 funky number. 32.').assert == '41443643532'
```