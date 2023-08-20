[<img align = right height = 50 width = 50 src = https://cdn4.iconfinder.com/data/icons/social-media-and-logos-11/32/Logo_Youtube-512.png>](https://youtu.be/q3Xlzg2Gj_c)
### # Task 2: Use JPMorgan Chase & Co. frameworks and tools
Aim: Implement the Perspective open source code in preparation for data visualisation

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
git clone https://github.com/theforage/forage-jpmc-swe-task-2
cd JPMC-tech-task-2-PY3
```

#### Correct code of App.tsk
```ruby
import React, { Component } from 'react';
import DataStreamer, { ServerRespond } from './DataStreamer';
import Graph from './Graph';
import './App.css';
 
/**
 * State declaration for <App />
 */
interface IState {
  data: ServerRespond[],
  showGraph: boolean,
}
 
/**
 * The parent element of the react app.
 * It renders title, button and Graph react element.
 */
class App extends Component<{}, IState> {
  constructor(props: {}) {
    super(props);
 
    this.state = {
      // data saves the server responds.
      // We use this state to parse data down to the child element (Graph) as element property
      data: [],
      showGraph: false,
    };
  }
 
  /**
   * Render Graph react component with state.data parse as property data
   */
  renderGraph() {
    if (this.state.showGraph){
      return (<Graph data={this.state.data}/>)
    }
  }
 
  /**
   * Get new data from server and update the state with the new data
   */
  getDataFromServer() {
    let x=0;
    const interval =setInterval(() => {
    DataStreamer.getData((serverResponds: ServerRespond[]) => {
      // Update the state by creating a new array of data that consists of
      // Previous data in the state and the new data from server
      this.setState({
        data: serverResponds,
        showGraph: true,
 
    });
  });
  x++;
  if (x>1000){
    clearInterval(interval);
  }
},100);
  }
 
  /**
   * Render the App react component
   */
  render() {
    return (
      <div className="App">
        <header className="App-header">
          Bank & Merge Co Task 2
        </header>
        <div className="App-content">
          <button className="btn btn-primary Stream-button"
            // when button is click, our react app tries to request
            // new data from the server.
            // As part of your task, update the getDataFromServer() function
            // to keep requesting the data every 100ms until the app is closed
            // or the server does not return anymore data.
            onClick={() => {this.getDataFromServer()}}>
            Start Streaming Data
          </button>
          <div className="Graph">
            {this.renderGraph()}
          </div>
        </div>
      </div>
    )
  }
}
 
export default App;
```

#### Correct code of Graph.tsk
```ruby
import React, { Component } from 'react';
import { Table } from '@finos/perspective';
import { ServerRespond } from './DataStreamer';
import './Graph.css';
 
/**
 * Props declaration for <Graph />
 */
interface IProps {
  data: ServerRespond[],
}
 
/**
 * Perspective library adds load to HTMLElement prototype.
 * This interface acts as a wrapper for Typescript compiler.
 */
interface PerspectiveViewerElement extends HTMLElement {
  load: (table: Table) => void,
}
 
/**
 * React component that renders Perspective based on data
 * parsed from its parent through data property.
 */
class Graph extends Component<IProps, {}> {
  // Perspective table
  table: Table | undefined;
 
  render() {
    return React.createElement('perspective-viewer');
  }
 
  componentDidMount() {
    // Get element to attach the table from the DOM.
    const elem= document.getElementsByTagName('perspective-viewer')[0] as unknown as PerspectiveViewerElement;
    const schema = {
      stock: 'string',
      top_ask_price: 'float',
      top_bid_price: 'float',
      timestamp: 'date',
    };
 
    if (window.perspective && window.perspective.worker()) {
      this.table = window.perspective.worker().table(schema);
    }
    if (this.table) {
      // Load the `table` in the `<perspective-viewer>` DOM reference.
 
      // Add more Perspective configurations here.
      elem.load(this.table);
      elem.setAttribute('view','y_line');
      elem.setAttribute('column-pivots','["stock"]');
      elem.setAttribute('row-pivots','["timestamp"]');
      elem.setAttribute('columns','["top_ask_price"]');
      elem.setAttribute('aggregates','{"stock":"distinct count","top_ask_price":"avg","top_bid_price":"avg","timestamp":"distinct count"}');
    }
  }
 
  componentDidUpdate() {
    // Everytime the data props is updated, insert the data into Perspective table
    if (this.table) {
      // As part of the task, you need to fix the way we update the data props to
      // avoid inserting duplicated entries into Perspective table again.
      this.table.update(this.props.data.map((el: any) => {
        // Format the data from ServerRespond to the schema
        return {
          stock: el.stock,
          top_ask_price: el.top_ask && el.top_ask.price || 0,
          top_bid_price: el.top_bid && el.top_bid.price || 0,
          timestamp: el.timestamp,
        };
      }));
    }
  }
}
 
export default Graph;
```

#### Run the server app is on Administrator Mode :
```ruby
cd JPMC-tech-task-2-PY3
python datafeed/server3.py
```

#### Run the client app is on Command Prompt :
```ruby
cd JPMC-tech-task-2-PY3
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
