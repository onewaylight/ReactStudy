```js
import { format } from 'date-fns';

        ...


        const nd = new Date(_values['until']);
        const cnvnd = format(nd, "yyyy/MM/dd");

        _values['until'] = cnvnd;

        const rg = edregdatetime as Date;
        const cnvrg = format(rg, "yyyy/MM/dd hh:mm:ss");

        _values['duedate'] = cnvnd;
        _values['regofdate'] = cnvrg;

```
