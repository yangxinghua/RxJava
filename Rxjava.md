# 关于RxJava

## 角色
* Observables
* Observer
* Subscribers
* Subjects

Observables和Subjects是两个“生产”实体。Observer和Subsribers是两个“消费”实体。

## “热”Observables和“冷”Observables
"热"Observables，一建立，就在发出事件，订阅者有可能会错过某些事件。“冷”Observables只有在有订阅都订阅之后，才会发出事件，订阅都可以收到全部事件。

## 创建Observable

### Observable.create()

```  

    private void testCreate() {
        Observer<String> observer = new Observer<String>() {
            @Override
            public void onCompleted() {
                Log.d(TAG, "onCompleted>>>>>>");
            }

            @Override
            public void onError(Throwable throwable) {
                Log.d(TAG, "onError>>>>>>>");
            }

            @Override
            public void onNext(String s) {
                mTextView.setText(s);
            }
        };


        Observable<String> observable = Observable.create(new Observable.OnSubscribe<String>() {
            @Override
            public void call(Subscriber<? super String> subscriber) {
                subscriber.onNext("1");
                subscriber.onNext("2");
                subscriber.onNext("3");
                subscriber.onNext("4");
                subscriber.onNext("5");
                subscriber.onCompleted();
            }
        });

        observable.subscribe(observer);
    }  


```

### Observable.from()

如果现在是一个序列，我们可以通过Observable.from()来发射每一个item而不是通过foreach来遍历再发射。

```
    private void testCreate() {
        Observer<String> observer = new Observer<String>() {
            @Override
            public void onCompleted() {
                Log.d(TAG, "onCompleted>>>>>>");
            }

            @Override
            public void onError(Throwable throwable) {
                Log.d(TAG, "onError>>>>>>>");
            }

            @Override
            public void onNext(String s) {
                mTextView.setText(s);
            }
        };


        Observable<String> observable = Observable.create(new Observable.OnSubscribe<String>() {
            @Override
            public void call(Subscriber<? super String> subscriber) {
                subscriber.onNext("1");
                subscriber.onNext("2");
                subscriber.onNext("3");
                subscriber.onNext("4");
                subscriber.onNext("5");
                subscriber.onCompleted();
            }
        });

        observable.subscribe(observer);
    }

```

### Observable.just()

如果我们已经定义了一个方法，可以通过just来转变为Observable. just方法可以传入1到9个参数，它会按照传入的顺序来发射它们。也可以传入数组或者list,不过不会发射每个元素，而是发射整个数组或者列表。

```
    private void testJust() {
        Observable<String> observable = Observable.just(helloworld());

        Observer<String> observer = new Observer<String>() {
            @Override
            public void onCompleted() {
                Logger.d(this, "onCompleted");
            }

            @Override
            public void onError(Throwable e) {
                Logger.d(this, "onError >>>>> " + e.getMessage());
            }

            @Override
            public void onNext(String s) {
                Logger.d(this, s);
            }
        };

        observable.subscribe(observer);
    }

    private String helloworld() {
        return "helloworld";
    }

```

### Observable.empty(), Observable.throw(), Observable.never();
当需要Observable不再发射数据并正常结束，可以使用empty. 当需要一个不发射数据并以错误结束时，可以使用throw.当需要一个不会发射数据也不会结束的Observable,可以使用nerver.
