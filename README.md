# univalid

Universal validation module. It may use different strategies for client validation, server validation and more.

In current moment exists two strategies:

- [univalid-strategy-default](https://github.com/StetsD/univalid-strategy-default) (use by default)
- [univalid-strategy-form](https://github.com/StetsD/univalid-strategy-form)

## Install

```sh
npm i univalid
```


## Base usage

```js
const Univalid = require('univalid');
const univalid = Univalid();

univalid.check([
    {
        name: 'login',
        val: 'User01',
        type: 'required'
    },
    {
        name: 'email',
        val: 'test@test.ts',
        type: 'email'
    },
    {
        name: 'password',
        val: undefined,
        type: 'password'
    }
]);

console.log(univalid.getCommonState, univalid.getState);
univalid.clearState();

```


## API


### check(pack)

Validating the pack

**pack** - Type `object`

Structure of pack must be strict.

- packItem.name - Type `string` - (required) - filed name
- packItem.type - Type `string` - (required) - by default has: 'required', 'email', 'password', 'equal'
- packItem.val - Type `string` - (required) value of field
- packItem.filter - Type `boolean` - filter type (see more [univalid-strategy](https://github.com/StetsD/univalid-strategy))
- packItem.msg - Type `boolean` - message config. See in example below

name, val, type - required fields
```js
//name, val, type - required fields

univalid.check(
    [
        {
            name: 'username',
            val: 'Uriy',
            type: 'required',
            filter: 'oL',
            msg: {
                empty: 'You shall not pass',
                invalid: 'Validation error',
                filter: 'Filter error',
                success: 'All right'
            }
        },
        {
            name: 'email',
            val: 'Uriy@mzf.com',
            type: 'email',
            filter: val => {
                // Your custom filter
                
                console.log('Filter', val);
                
                // if FilterHandler is Ok then "return true"
                    return true;
                // else return false
            },
            msg: {
                empty: 'You shall not pass',
                invalid: 'Bad email',
                filter: 'Only lat/numbers/specials symbols',
                success: 'All right'
            }
        }
    ]
);

```


### setStrategy(strategy)

Set new Strategy of validation

**strategy** - Type `object` - instance of strategy

```js
const UnivalidStrategyForm = require('univalid-strategy-form');

univalid.setStrategy(
    UnivalidStrategyForm({
        core: univalid, /* required prop */
        $form: '.js-reg-form' /* required prop */
    })
);
```


### setValidHandler(pack)

Set new Validation Handler

**pack** - Type `object`

New validationHandler must return true\false how result validation of field

```js
univalid.setValidHandler({
    'newValidator': val => {
        console.log(val, 'Valid');
        return true;
    }
});
```


### setMsgConfig(config)

Set new  Default Message config

If in item of validation pack not define 'msg' field, will be message from msgConfig be default 

**config** - Type `object`

```js
univalid.setMsgConfig({
    empty: 'NEW EMPTY ERROR', 
    invalid: 'NEW INVALID', 
    filter: "NEW FILTER", 
    success: 'NEW SUCCESS'
});
```


### toggleDefaultMsgConfig()

Toggle to default and common configuration of messages.

This configuration is common for all univalid modules.

```js
univalid.toggleDefaultMsgConfig(); // default msgConfig
univalid.toggleDefaultMsgConfig(); // msgConfig of instance
```


### setDefaultMsgConfig(config)

Set new Common Message config 

**config** - Type `object`

```js
univalid.setMsgConfig({
    empty: 'NEW COMMON EMPTY ERROR', 
    invalid: 'NEW COMMON INVALID', 
    filter: "NEW COMMON FILTER", 
    success: 'NEW COMMON SUCCESS'
});

//or

univalid.setMsgConfig({
    empty: 'NEW COMMON EMPTY ERROR'
});
 
```


### set(option, val)

Set new prop to your current strategy of validation 

**option** - Type `string`

```js
univalid.set('core', univalid);
```


### get(prop, args)

Get prop your current strategy or call the method your strategy.  

**prop** - Type `string`

**args** - if it a method of strategy

```js

//univalid-strategy-form example

univalid.get('addEvent', {
    newEvent(){document.addEventListener('click', ()=>{
	    console.log('Click in document!');
    })}
});

univalid.get('clsConfig');

```


### clearState()

Clear your current validation state


### getState()

Get last validation state


### getStrategy()

Get current Strategy of validation


### getValidHandler()

Get current validation handler


### getCommonState()

Get Common state of validation (true\false)



## EVENTS

You can subscribe on univalid events (univalid extends EventEmitter)

```js

univalid.on('start:valid', (args) => {
    console.log('Check!');
});

```

**Table of events**

| Event | Description |
|:------:|:-----------:|
|start:valid|Start validation pack|
|end:valid|End validation pack|
|start:valid:field|Start validation field|
|end:valid:field|End validation field|
|change:strategy|Change strategy event|
|set:new-ValidationHandler|Set new ValidationHandler event|
|change:msg-config|Change message config event|
|clear:state|Clear state of last validation event|
|error|Error event|


## License
ISC ©
