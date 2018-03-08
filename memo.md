# ユニットテストで考えるべきこと
仕様を**最小限**の言葉で伝える.  

悪い設計のコードに絆創膏を貼るためではない.  
それはノイズになり,後からこれを読んだ人に余計な情報を与えることになる.

## 良くない例
`minTime`という関数があり、これのユニットテストを書くとする.  

```
let hoge = new MockHoge();  
let cases = [
        generateHogeArgs([{time: 1}]),
        generateHogeArgs([{time: 1},{time: 2},{time: 3}]),
        generateHogeArgs([{time: 3},{time: 2},{time: 1}]),
        generateHogeArgs([{time: 3},{time: 1},{time: 2}])
    ]);
    
for(let case of cases){
    assertEqual(1, hoge.minTime(case));
}
```
このようなテストを書いたとする.  
これを読んだ人には以下のような疑問が浮かぶ.

- `minTime`という名前なのに,要素が1つの時と複数の時を分けている…これは何が意味があるのか？
- `minTime`という名前なのに,昇順,降順,ランダムを網羅している…以下略

例えばこれ以外にも,

- generatorを使わずに`minTime`が要求する`args`を直書きする.(time以外のプロパティは使わないという前提)   
  どのプロパティを使っているのかが伝わらない

等があげられる.

## 良い例
```
let hoge = new MockHoge();  
assertEqual(1, hoge.minTime({time: 1},{time: 2},{time: 3}));
```

あるいは,`1と4が与えられたら,4を返す`という仕様が`minTime`にあった場合は、`minTime`を直すか.
```
let hoge = new MockHoge();  
assertEqual(1, hoge.minTime({time: 1},{time: 2},{time: 3}));
assertEqual(1, hoge.minTime({time: 1},{time: 2},{time: 4})); // 1と4が与えられたら,4を返す
```

といった具合にコメントで補足するのもあり.
