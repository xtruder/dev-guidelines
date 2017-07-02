# NodeJs

> NodeJS is the only real dev language

## General

- interpeter: nodejs >= 7.6
- coding standard: ES7
- filestyle: uft-8, 4 spaces, lf, trim trailing whitespaces, final newline
- codestyle: xo/esnext

### NodeJS Installation

Follow instructions on https://nodejs.org/en/download/package-manager/

### Tooling

- Package manager: yarn, npm
- Package registry: sinopia
- Release: np
- Typecheck: flowtype
- Compiler: babel
- Scaffolding: yeoman

### Libraries

#### Runtime

- Logging: microkit, winston
- HTTP client: request-promise
- HTTP server: koa@next
- Amqp: amqplib

#### Testing

- Testing framework: mocha
- Expect framework: chai
- Mock: sinon
- HTTP mock: nock

## Programmer notes

We still see many developers writing ancient es5 javascript code. In a world where
we have node 7, that supports almost all es6 and even some features of es7, it's
 to write in code style of internet explorer 8. Even for ancient interpreters,
there are compilers that can transpile code to version of javascript that is needed.
While it's understandable that not all bleading features should be used, it's
ok 

### Avoid callback pattern

Promises exists in javascript from nodejs 0.12. That's even before node.js
had real versioning and looked like unstable beta. There are still many libraries
that use callback pattern, these are usually old libraries, that you should
avoid.

If you have to deal with some code that still uses callback, you can wrap
code with a promise:

```
new Promise((resolve, reject) => {
  my_ancient_function(err => err ? reject(err) : resolve());
});
```

### Avoid lodash/underscore when possible

Some people are still big fans of underscore like libraries which provided framework
for working with javascript structures, it turns out that es6 and es7 get much
better internal support for working with js structures. There are two main reasons
why avoid usage of these: better debugging and less code breakage. For example
lodash changed a lot of methods between versions. In one major update the code
broken, because methods did not exist anymore, and no deprecation warning was
raised.

Example of ES6/ES7 methods that you can use:

- list.filter
- list.find
- list.forEach
- list.map
- list.recude
- list.includes
- Object.assign
...

These internal javascript methods are much faster, and provide nicer stack
traces which allows easier debugging.

### Avoid promise libraries when possible

NodeJS has internal support for promises. Using internal promises, will not give
you such rich functionality, but will provide much better debugging support and
nicer stack traces.

### Avoid compiling javascript if not needed

I know there are fancy bleeding es7 or es8 features that you want to abuse, or you really want
to use that typescript, that you have no idea how to use right, as you will soon be just writing
normal javascript. Please don't, use native node.js 7 features instead. While your code won't be
totaly bleeding edge, you will at least have some sane ways to debug. And i know you are master
`console.log` debugging guru, your sysadmin is not. When you will beg him to debug this mistery
bug in production, the only thing he will have on his dispose is a debugger. He will be much happier
if he would actually be able to read code.

If you need to compile, use at least decent compiler, like typescript. Babel produces search and
replace garbage.

### Use async/await

Async/await is a new promise syntastic sugar that will get rid of ugly promises and replace with
something that async language like javascript should have from day one. It's supported in all new
node.js versions. Even if your production does not have node.js 7, you can use
babel to compile.

Unuglify example:

```
// orders all cheese pizza
Restaurant.get(id).then(restaurant => {
  return Menu.query(restaurant.id).then(items => {
    return Promise.all(items.map(item => {
      if (item.kind === 'pizza') {
        return Ingiridients.getForItem(item.id).then(ingiridents => {
          for (const ingridient of ingiridents) {
            if (ingridient.name === 'cheese') {
              return Restaurant.order(item);
            }
          }
        });
      }
    }));
  });
});
```

To this: 

```
const restaurant = await Restaurant.get(id);
const items = await Menu.query(restaurant.id);
const pizzas = items.filter(item => item.kind === 'pizza');

await Promise.all(pizzas.map(async pizza => {
   const ingridients = await Ingiridients.getForItem(pizza.id);
   
   if (ingridents.find(ingridient => ingiridient.name === 'cheese')) {
     await Restaurant.order(pizza);
   }
}));
```

Much nicer!

### Use classes instead of direct `module.exports`:

Classes provide greater flexibility when compared to direct `module.exports`
and are not fundamendaly any harder. Here is an example:

```
const users = {};

module.exports = 
    create(model) {
        users[model.id] = model;
    }

    get(id) {
        return users[id];
    }
};
```

Compared to:

```
class UserRepository extends Repository {
    constructor() {
        super();

        this._users = {};
    }

    create(model) {
        this._users[model.id] = model;
    }

    get(id) {
        return this._users[id];
    }
}

module.exports = UserRepository;
```

With classes you can do so much more. First you can inherit from them, you can
define getters and setters, you can instantiate them and prodvide class constructor,
you can define static methods.
If you use `module.exports` direcyly, you can hack all of that, but this is
really bad practice in ES6 forward.

Some developers think composition over inheritance. Composition is just a hip
word for class mixins. While javascript, can't inherit from multiple bases, you
can use functions that construct class mixins:

```
function loggerMixin(cls) {
    return class extends cls {
        constructor() {
            super();

            this._logger = new Logger(this.constructor.name);
        }
    };
}

function errorReporterMixin(cls) {
    return class extends cls {
        constructor() {
            super();

            this._logger = new ErrorReporter(this.constructor.name);
        }
    };
}

class Repository {
    static mixin(mixinFunc) {
        return mixinFunc(this);
    }
}

class UserRepository extends Repository
    .mixin(loggerMixin)
    .mixin(errorReporterMixin) {

}
```

This is not some magic construct, but the same is also used in some frameworks.
