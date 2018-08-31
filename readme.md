# vue-json-view

## Installation

```shell
npm install vuui-json-view
```

## Usage
```javascript
import JsonView from "vuui-json-view"

export default {
    data () {
        return {
            jsonSource: [
                {
                    a: 111,
                    b: 'bbb',
                    c: {
                        c1: 'c1',
                        c2: [1, 2, 3]
                    }
                },
                2,
                3
            ]
        }
    },
    components: {
        JsonView
    }
}
```


```html
<div>
  <json-view :json="jsonSource"></json-view>
</div>
```

## Changelog
* 1.0.0: Initial Release