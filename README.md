#  CreateWidjet

## DESCRIPTION

Createwidjet DEMO is a basic app useful to show how to use a simple Widjet (helpfull for beginner)
In this current version from the MainActivity the User can add a FAKE book into the DB (It contains a casual real author name)
Automatically after every add-action the widjet will be updated by displaing the added author also

IN FEW WORDS: widjet displaies authors in DB and it is updated after insert into DB

## Installation

Download zip file project and deploy it on Android Virtual device (AVD) or Smartphone/tablet for example with VISUAL STUDIO

## Dependencies

Room dependencies

`
def room_version = '2.0.0-beta01'
    implementation "androidx.room:room-runtime:$room_version"
    annotationProcessor "androidx.room:room-compiler:$room_version"
    
`

## General Info

Data Access Object class (Method-Query mapping)

`

@Dao
public interface DaoStarBook {
    @Query("select id_book  from book")
    List<String> getAllIdBookFromFavourite();

    @Insert
    void insertBook(StarBook starBook);

    @Update(onConflict = OnConflictStrategy.REPLACE)
    void updateBook(StarBook starBook);

    @Delete
    void deleteBook(StarBook starBook);

    @Query("select *  from book ")
    List<StarBook>  loadFavouriteBooks();

    @Query("delete from book where id_book =:idbook")
    void deleteBookById(String idbook);

    @Query("select *  from book where id_book =:idbook")
    List<StarBook>  loadBookWithId(String idbook);

    @Query("select *  from book")
    Cursor loadWidjetFavouriteBooks();
}

`

In MainActivity

Method used to add and update widjets


`

public void addBook(){

        btn = findViewById(R.id.add_btn);
        btn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                AppExecutors.getInstance().diskIO().execute(new Runnable() {
                    @Override
                    public void run() {
                        //Fake book with casual author
                        StarBook s = new StarBook(FAKE_ID, FAKE_TITLE, FAKE_DESCRIPTION,FAKE_PICTURE_LINK, FAKE_AUDIO_LINK  , authors[generateCasualAuthorIndex(0,authors.length-1)]+", "  );
                        myDb.BookDAO().insertBook( s );

                        //UPDATE WIDJET
                        Intent intent = new Intent(MainActivity.this, NewAppWidget.class);
                        intent.setAction("android.appwidget.action.APPWIDGET_UPDATE");
                        int ids[] = AppWidgetManager.getInstance(getApplication()).getAppWidgetIds(new ComponentName(getApplication(), NewAppWidget.class));

                        intent.putExtra(AppWidgetManager.EXTRA_APPWIDGET_IDS,ids);
                        sendBroadcast(intent);
                    }
                });
            }

        });

    }

`

## Next step

Remove duplicate authors from widjet

## License

Public Domain Work. Download the code and go haead to do whatever you want :)
