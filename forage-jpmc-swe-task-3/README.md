[<img align = right height = 50 width = 50 src = https://cdn4.iconfinder.com/data/icons/social-media-and-logos-11/32/Logo_Youtube-512.png>](https://youtu.be/NCmYZhnNYzc)
### # Task 3: Display data visually for traders
Aim: Use Perspective to create a chart for a trading dashboard

#### Prerequisite
- [Python Installation](https://iamvishalprasad.blogspot.com/2023/06/python-installation.html#more)
- [PyCharm Installation](https://iamvishalprasad.blogspot.com/2023/06/pycharm-installation.html#more)
- [Git Installation](https://iamvishalprasad.blogspot.com/2023/06/git-installation.html#more)
- [Visual Studio Code Installation](https://iamvishalprasad.blogspot.com/2023/06/visual-studio-code-installation.html#more)
- [Nodejs Installation](https://iamvishalprasad.blogspot.com/2023/07/nodejs-installing.html#more)
- [NVM Installation](https://iamvishalprasad.blogspot.com/2023/07/nvm-installing.html#more)
- Visual C++ Build Environment Installation via Visual Studio Build Tools.

#### Visual Studio Installer Installation then run command
```ruby
npm config set msvs_version 2017
```

#### Install the right node version using nvm and that would consequently get us the right npm version as well.
```ruby
nvm install v11.0.0
nvm use v11.0.0
```

#### Download the latest version of the npm 
```ruby
run npm -install
npm --global
```

#### Clone your project using Command Prompt:
```ruby
git clone https://github.com/theforage/forage-jpmc-swe-task-3
cd JPMC-tech-task-3-PY3
```
#### Correct code of Data Manipulator.ts
```ruby
import { ServerRespond } from './DataStreamer';
 
export interface Row {
  [key: string]: string|any;
  price_abc: number,
  price_def: number,
  ratio: number,
  timestamp: Date,
  upper_bound: number,
  lower_bound: number,
  trigger_alert: number | undefined,
}
 
 
export class DataManipulator {
  static generateRow(serverRespond: ServerRespond[]): Row {
    const priceABC= (serverRespond[0].top_ask.price+serverRespond[0].top_bid.price)/2;
    const priceDEF= (serverRespond[1].top_ask.price+serverRespond[1].top_bid.price)/2;
    const ratio = priceABC/priceDEF;
    const upperBound=1+0.05;
    const lowerBound=1-0.05;
    return {
      price_abc: priceABC,
      price_def: priceDEF,
      ratio,
      timestamp: serverRespond[0].timestamp > serverRespond[1].timestamp ?
        serverRespond[0].timestamp : serverRespond[1].timestamp,
      upper_bound: upperBound,
      lower_bound: lowerBound,
      trigger_alert: (ratio > upperBound || ratio < lowerBound) ? ratio : undefined,
 
    };
  }
}
```

#### Correct code of Graph.tsx
```ruby
import React, { Component } from 'react';
import { Table } from '@finos/perspective';
import { ServerRespond } from './DataStreamer';
import { DataManipulator } from './DataManipulator';
import './Graph.css';
 
interface IProps {
  data: ServerRespond[],
}
 
interface PerspectiveViewerElement extends HTMLElement {
  load: (table: Table) => void,
}
class Graph extends Component<IProps, {}> {
  table: Table | undefined;
 
  render() {
    return React.createElement('perspective-viewer');
  }
 
  componentDidMount() {
    // Get element from the DOM.
    const elem = document.getElementsByTagName('perspective-viewer')[0] as unknown as PerspectiveViewerElement;
 
    const schema = {
      price_abc: 'float',
      price_def: 'float',
      ratio: 'float',
      timestamp: 'date',
      upper_bound: 'float',
      lower_bound: 'float',
      trigger_alert: 'float',
    };
 
    if (window.perspective && window.perspective.worker()) {
      this.table = window.perspective.worker().table(schema);
    }
    if (this.table) {
      // Load the `table` in the `<perspective-viewer>` DOM reference.
      elem.load(this.table);
      elem.setAttribute('view', 'y_line');
      elem.setAttribute('row-pivots', '["timestamp"]');
      elem.setAttribute('columns', '["ratio", "lower_bound", "upper_bound", "trigger_alert"]');
      elem.setAttribute('aggregates', JSON.stringify({
        price_abc: 'avg',
        price_def: 'avg',
        ratio: 'avg',
        timestamp: 'distinct count',
        upper_bound: 'avg',
        lower_bound: 'avg',
        trigger_alert: 'avg',
      }));
    }
  }
 
  componentDidUpdate() {
    if (this.table) {
      this.table.update([
        DataManipulator.generateRow(this.props.data),
      ]);
    }
  }
}
 
export default Graph;
```

#### Run the server app is on Administrator Mode :
```ruby
cd JPMC-tech-task-3-PY3
python datafeed/server3.py
```

#### Run the client app is on Command Prompt:
```ruby
cd JPMC-tech-task-3-PY3
npm start
```

#### To make patch file
```ruby
- git add -A
- git config user.email "<your_email_address>"
- git config user.name "<your_name>"
- git commit -m "Create Patch File"
- git format-patch -1 HEAD
```
