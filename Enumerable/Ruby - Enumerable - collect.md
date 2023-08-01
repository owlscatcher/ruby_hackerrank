## Problem

Beside simple methods to iterate over objects, Ruby `Enumerable` provides a number of important higher order constructs that we shall explore in further challenges. One of such important methods is `collect` method, also known as `map`.

`map` as the name may suggest, takes a function and maps (applies) it to a collection of values one by one and returns the collection of result.

That is, $map(f(x), [x_{1}, x_{2}, x_{3}, \dots, x_{n}]) \to [f(x_{1}, f(x_{2}, \dots, f(x_{n})))$

![[Pasted image 20230731004743.png]]
This single powerful method helps us to operate on a large number of values at once.

For example,

```ruby
>>> [1,2,3].map { |x| 2*x }
=> [2, 4, 6]
>>> {:a=>1, :b=>2, :c=>3}.collect { |key, value| 2*value }
=> [2, 4, 6]
```

Note that these methods are different from `each` in the respect that these methods return a **new** collection while former returns the original collection, irrespective of whatever happens inside the block.

In this challenge, your task is to write a method which takes an _array of strings_ (containing secret enemy message bits!) and decodes its elements using [ROT13](http://en.wikipedia.org/wiki/ROT13) cipher system; returning an array containing the final messages.

For example, this is how ROT13 algorithm works,

Original text:
```bash
Why did the chicken cross the road?
Gb trg gb gur bgure fvqr!
```

On application of ROT13,
```bash
Jul qvq gur puvpxra pebff gur ebnq?
To get to the other side!
```

## Solution

Так как в параметры передают массив сообщений:
```ruby
secret_messages = [
	["Why did the chicken cross the road?"],
	["Gb trg gb gur bgure fvqr!"]
]
```

Мы проходим по этому массиву циклом, внутри которого будет работать еще один цикл:

```ruby
secret_messages.map do |msg|
  msg.char.map { any do}
end
```

Нам приходится  преобразовать строку в массив символов при помощи метода `.char()`, потому что мы не можем использовтаь `.map()` по строке, так как `.map()` используется только на перечислениях, потому мы и приводим строку к массиву символов.

Чтобы нам было удобнее работать с таблицей шифрования, мы создаем два массива и объединяем их в хеш в методе `create_crypt_hash`, где ключом является исходный символ, а значением его зашифрованное значение.

Итог:

```ruby
def create_crypt_hash()
  arr00 = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz ?!,.;:-".chars
  arr13 = "NOPQRSTUVWXYZABCDEFGHIJKLMnopqrstuvwxyzabcdefghijklm ?!,.;:-".chars
  
  secret_hash = Hash.new
  arr00.size.times { |index| secret_hash.store(arr00[index], arr13[index]) }

  secret_hash
end

def rot13(secret_messages)
  # your code here
  secret_hash = create_crypt_hash()
  
  secret_messages.map do |msg_arr|
    msg_arr.chars.map { |ch| ch = secret_hash[ch] }.join
  end
end
```
