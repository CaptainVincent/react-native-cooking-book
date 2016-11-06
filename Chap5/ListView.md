# ListView

ListView 需要指名兩個屬性, 分別是 dataSource 說明呈現的內容來源, renderRow 如何根據內容去渲染的方式。
```javascript
<ListView
  dataSource={this.state.dataSource}
  renderRow={this._renderRow}
  />
```

而 ListView.DataSource 又需要時做 rowHasChanged 的判斷方法, 底下是個 Simple Case
```javascript
ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
```

## BookLisk Demo
```
./
|-- index.ios.js //React code
|-- index.android.js //React code
|-- BookItem.js //New file
|-- BookList.js //New file
```

(index.ios.js 省略介紹)

###BookList.js
如何透過 NY Times 的 API 取得資料更新 ListView 中的 content
```javascript
import React, {
  Component,
} from 'react';

import {
  StyleSheet,
  Text,
  View,
  Image,
  ListView,
} from 'react-native';

import BookItem from './BookItem';

const API_KEY = '73b19491b83909c7e07016f4bb4644f9:2:60667290';
const QUERY_TYPE = 'hardcover-fiction';
const API_STEM = 'http://api.nytimes.com/svc/books/v3/lists';
const ENDPOINT = `${API_STEM}/${QUERY_TYPE}?response-format=json&api-key=${API_KEY}`;

class BookList extends Component {
  constructor(props) {
    super(props);
    var ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
    this.state = {
      dataSource: ds.cloneWithRows([])
    };
  }

  componentDidMount() {
    this._refreshData();
  }

  _renderRow(rowData) {
    return <BookItem coverURL={rowData.book_image} title={rowData.title} author={rowData.author}/>;
  }

  _renderHeader() {
    return (<View style={styles.sectionDivider}>
      <Text style={styles.headingText}>
        Bestsellers in Hardcover Fiction
      </Text>
      </View>);
  }

  _renderFooter() {
    return(
      <View style={styles.sectionDivider}>
        <Text>Data from the New York Times bestsellers list.</Text>
      </View>
      );
  }

  _refreshData() {
    fetch(ENDPOINT)
      .then((response) => response.json())
      .then((rjson) => {
        this.setState({
          dataSource: this.state.dataSource.cloneWithRows(rjson.results.books)
        });
      });
  }

  render() {
    return (
        <ListView
          style={{marginTop: 24}}
          dataSource={this.state.dataSource}
          renderRow={this._renderRow}
          renderHeader={this._renderHeader}
          renderFooter={this._renderFooter}
          enableEmptySections={true}
          />
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#FFFFFF',
    paddingTop: 24
  },
  list: {
    flex: 1,
    flexDirection: 'row'
  },
  listContent: {
    flex: 1,
    flexDirection: 'column'
  },
  row: {
    flex: 1,
    fontSize: 24,
    padding: 42,
    borderWidth: 1,
    borderColor: '#DDDDDD'
  },
  sectionDivider: {
    padding: 8,
    backgroundColor: '#EEEEEE',
    alignItems: 'center'
  },
  headingText: {
    flex: 1,
    fontSize: 24,
    alignSelf: 'center'
  }
});

export default BookList;
```

###BookItem.js
定義如何呈現列表內元素

```javascript
import React, {
  Component,
} from 'react';

import {
  StyleSheet,
  Text,
  View,
  Image,
  ListView,
} from 'react-native';


const styles = StyleSheet.create({
  bookItem: {
    flex: 1,
    flexDirection: 'row',
    backgroundColor: '#FFFFFF',
    borderBottomColor: '#AAAAAA',
    borderBottomWidth: 2,
    padding: 5
  },
  cover: {
    flex: 1,
    height: 150,
    resizeMode: 'contain'
  },
  info: {
    flex: 3,
    alignItems: 'flex-end',
    flexDirection: 'column',
    alignSelf: 'center',
    padding: 20
  },
  author: {
    fontSize: 18
  },
  title: {
    fontSize: 18,
    fontWeight: 'bold'
  }
});

class BookItem extends Component {
  propTypes: {
    coverURL: React.PropTypes.string.isRequired,
    author: React.PropTypes.string.isRequired,
    title: React.PropTypes.string.isRequired
  }

  render() {
    return (
      <View style={styles.bookItem}>
        <Image style={styles.cover} source={{uri: this.props.coverURL}}/>
        <View style={styles.info}>
          <Text style={styles.author}>{this.props.author}</Text>
          <Text style={styles.title}>{this.props.title}</Text>
        </View>
      </View>
      );
  }
}

export default BookItem;
```