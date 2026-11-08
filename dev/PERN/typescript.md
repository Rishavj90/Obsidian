# basic commands

``` shell
tsc --init 
```
- for setting up tsconfig
```shell
tsc -W 
```
- for watch mode
```shell
tsx watch <file>
```
- like nodemon 

# more

```ts
console.log("hello")
let user = {
    name : "rishav",
    age : 10
}
const myname:string = "namee"

// passing default values 
function add(a:number, b:number=8):number {
    return a+b
}
console.log(add(4))

// type alias
type user = {
    readonly name : string  //can't update it
    class? : string  // makes it optional, so if u didn't pass the value, that'll be fine
} 

// merging types into one
type cardNumber = {
    cardnumber : string
}
type cardDate = {
    cardDate : string
}
type cardDetails = cardNumber & cardDate & {
    cvv : string
}

// definig arrays
let myarr : string[] & number[] = [] //wrong 
let myarr2 : (string | number)[] = []
myarr2.push("spidey")

// union types
let score : string | boolean | number
score = 5 

// tuples
let mytuple : [number, string, boolean]
mytuple = [4, "hello", true]
// mytuple = ["hello", true, 4] : wrong

// enums
const enum options{ //don't add const and generated js would be bonkers 
    one,
    two,
    three,
    four,
    five
}
let myrating = options.one

// interface
interface myInterface{
    name : string;
    readonly class : string;
    weight ?: number;
    myFunc() : boolean;
    myFunc2(friend: string): string; 
}

// interface vs type alias
//  -u can extend and reassign interface (that just adds the new value to initial interface)
interface myInterface{
    githubLogin : boolean
}
//  adds to one more value to interface
// inheritance
interface Admin extends myInterface{
    role : string
}
// for type alias it'll be like myInterface & {role : string}

// classes
class miClass{
    name : string = "";
    email : string = "";
    private _myNumber: number = 0;
    constructor(name:string, email: string){
        this.name = name
        this.email = email
    }
    // setters must not have any return type
    set returnNum(num:number){
        this._myNumber=num
    }
}

// abstract class
// can't create object
// can create object from class that inherits that "abstract class"
abstract class myabClass{
    abstract myFunc():void
    myFunc2() :void {
        console.log("hello")
    }
}

// Generics
function myGen<T>(val:T): T{
    return val
}
myGen("9")
myGen(6)
// myGen<Admin>({})

function getProperty<Type, Key extends keyof Type>(obj: Type, key: Key) {
  return obj[key];
}
 
let x = { a: 1, b: 2, c: 3, d: 4 };
 
getProperty(x, "a");

// Type Narrowing
// instanceof : only classes, No interface or type alias
function checkType(x:String|Date) :x is String {
    if(x instanceof String){
        return true
    }
    return false
}
```