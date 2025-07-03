# 1. String 객체란?

원시 타입인 문자열을 다룰 때 유용한 프로퍼티와 메서드를 제공한다.

수정이 불가능한 불변성 (immutable)을 가지며, 인덱스로 접근이 가능한 유사 배열이다.

# 2. 생성 방법

1. 문자열 리터럴 → typeof 하면 string

   ```jsx
   const str1 = 'hello'
   console.log(typeof str1) // string (원시 타입)
   ```

2. new String() 생성자 함수 → typeof 하면 object

   ```jsx
   const str2 = new String('hello')
   console.log(typeof str2) // object
   console.log(str2) // String { "hello" }
   ```

   > 🥲 **왜 `new String()`으로 생성하면 `typeof` 가 object인 거지?**
   >
   > > JS에는 원시 타입이 있고 객체 타입이 있는데
   > >
   > > 코드로 차이를 보면 아래와 같고
   > >
   > > ```jsx
   > > const str1 = 'hello' // 원시 타입 (string)
   > > const str2 = new String('hello') // 객체 타입 (String 객체)
   > > ```
   > >
   > > 따라서 new String은 객체를 생성하기 때문에 typeof 가 object임!

3. String() 함수 호출 → typeof 하면 string

   숫자, 불리언, null, undefined, 객체 등 → 문자열로 변환할 때 사용한다.

   ```jsx
   const str3 = String(123)
   console.log(str3) // "123"
   console.log(typeof str3) // string
   ```

# 3. 자주 쓰는 메서드

1. **slice(start, end)**

   부분 문자열을 반환하며 end는 포함되지 않는다.

   음수로 할 경우 뒤에서부터 문자열을 반환한다.

   ```jsx
   'hello world'.slice(0, 5) // 'hello'
   'hello world'.slice(-5) // 'world'
   ```

   > 🥲 `substring()`과 유사하지만, substring은 **음수가 불가하다!**

2. **toUpperCase(), toLowerCase()**

   ```jsx
   'Hello'.toUpperCase() // 'HELLO'
   'Hello'.toLowerCase() // 'hello'
   ```

3. **trim(), trimStart(), trimEnd()**

   공백 제거할 때 사용한다.

   ```jsx
   '  foo  '.trim() // 'foo'
   '  foo  '.trimStart() // 'foo  '
   '  foo  '.trimEnd() // '  foo'
   ```

4. **repeat(n)**

   소수로 입력하면 정수로 내려서 동작한다.

   0을 넣으면 빈 문자열을 반환하고, 음수를 입력하면 `RangeError`를 뱉는다.

   ```jsx
   'abc'.repeat(2) // 'abcabc'
   ```

5. **replace(pattern, newStr | fn)**

   첫 번째로 일치하는 문자열만 변경한다.

   ```jsx
   'Hello world'.replace('world', 'Hazel') // 'Hello Hazel'
   'Hello world'.replace(/world/, '<b>$&</b>') // 'Hello <b>world</b>'
   ```

6. **split(separator, limit)**

   ```jsx
   'How are you'.split(' ') // ['How', 'are', 'you']
   'Hello'.split('') // ['H', 'e', 'l', 'l', 'o']
   'How are you'.split(' ', 2) // ['How', 'are']
   ```
