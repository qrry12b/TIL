# Room을 사용하여 로컬 데이터베이스에 데이터 저장 (RxJava3)

## SQLite로 직접 작업하는것보다 Room 사용 시 이점
* 컴파일 시점에서 SQL 쿼리 검사
* 반복적이고 오류가 발생하기 쉬운 상용구 코드를 최소화 (어노테이션 등)
* 간소화된 데이터베이스 이전 경로

- - - - -

dependencies 추가
```
def room_version = "2.4.0"
implementation "androidx.room:room-runtime:$room_version"
annotationProcessor "androidx.room:room-compiler:$room_version"
implementation "androidx.room:room-rxjava3:$room_version"
implementation 'io.reactivex.rxjava3:rxjava:3.1.3'
implementation 'io.reactivex.rxjava3:rxandroid:3.0.0'
```

```
@Entity(tableName = "TB_MEMO")
public class Memo {
    @PrimaryKey(autoGenerate = true)
    @ColumnInfo(name = "UID")
    public long uid;

    @ColumnInfo(name = "TITLE")
    public String title;

    @ColumnInfo(name = "CONTENT")
    public String content;
}
```

```
@Dao
public interface MemoDao {
    @Insert(onConflict = OnConflictStrategy.REPLACE)
    public Completable insertMemo(Memo meom);

    @Update
    public Completable updateMemo(Memo meom);

    @Delete
    public Completable deleteMemo(Memo meom);

    @Query("SELECT * FROM TB_MEMO WHERE uid = :uid")
    public Single<Memo> selectMemo(long uid);

    @Query("SELECT * FROM TB_MEMO")
    public Single<List<Memo>> selectAllMemo();

    @Query("SELECT COUNT(uid) FROM TB_MEMO")
    public Single<Long> getCount();
}
```

```
@Database(entities = {Memo.class}, version = 1)
public abstract class AppDatabase extends RoomDatabase {
    public abstract MemoDao memoDao();
}
```

```
public class DatabaseBuilder {
    private static AppDatabase INSTANCE = null;
    public static AppDatabase getInstance(Context context) {
        if(INSTANCE == null) {
            synchronized (DatabaseBuilder.class) {
                if(INSTANCE == null) {
                    INSTANCE = Room.databaseBuilder(context, AppDatabase.class, "memo.db").fallbackToDestructiveMigration().build();
                }
            }
        }
        return INSTANCE;
    }
}
```

```
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    memoDao = DatabaseBuilder.getInstance(getApplicationContext()).memoDao();
    Memo memo = new Memo();
    memo.title = "title 1";
    memo.content = "memo content";
    memoDao.insertMemo(memo)
        .subscribeOn(Schedulers.io())
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(new CompletableObserver() {
            @Override
            public void onSubscribe(@NonNull Disposable d) {
                Log.d(TAG, "MainActivity.class onSubscribe");
            }

            @Override
            public void onComplete() {
                Toast.makeText(MainActivity.this, "onComplate", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onError(@NonNull Throwable e) {
                Toast.makeText(MainActivity.this, "onError", Toast.LENGTH_SHORT).show();
            }
        });
}
```

안드로이드 스튜디오 하단의 App Inspection 에서 테이블에 Insert된 데이터를 확인 할 수 있습니다.

### REF
* [MVNRepository](https://mvnrepository.com/artifact/io.reactivex.rxjava3/rxjava)
* [Android Docs](https://developer.android.com/training/data-storage/room?hl=ko)
* [Android Docs](https://developer.android.com/training/data-storage/room/migrating-db-versions?hl=ko)
* [Android Docs](https://developer.android.com/training/data-storage/room/async-queries?hl=ko)
* [Android Docs](https://developer.android.com/studio/inspect/database?utm_source=android-studio)