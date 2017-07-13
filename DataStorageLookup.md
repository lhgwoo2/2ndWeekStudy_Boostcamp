## 안드로이드 스튜디오 저장소 살펴보기

### 1. 프리퍼런스 활용

안드로이드에서 제공하는 저장소의 일종, xml로 데이터를 처리하기 때문에 동작속도가 느리다.
저장하였을 때 파일 위치는 data/data/[패키지 이름]/shared_prefs에 저장되며 저장되는 방식은 키이름(Keyname) - 저장된 값(value Pair)의 쌍으로 저장된다.

입력을 저장하는 방법을 생각해야 한다. 안드로이드는 데이터를 저장하고 시작화면을 만드는 방법을 제공하는데 그것이 프리퍼런스이다.

키 값은 String 형태여야 하며, Value는 Primitive 해야한다.
사용자에 대한 간단한 문장이나 숫자값을 저장하기 위해 사용된다.

프리퍼런스는 앱의 프리퍼런스를 저장하기 때문에 공유프리퍼런스라 불린다.

**프리퍼런스가 사용될만한 곳**

1. 환경설정 값 저장 
	 - 안드로이드의 다른 프레임워크와 협업이 필요(ex. 셋팅 액티비와 같은 -> 이런것을 Preference Fragment 라 한다.)
2. 액티비티간 정보 교환
3. 자동완성


**장점 & 단점**

[장점]

1. 사용법이 쉽다.
2. 액티비티간 정보교환에도 활용이 가능하다
3. 어느정도 안전한 저장법이다.
4. 다른 프로그램이 접근할 수 없다.

[단점]

1. 설정값을 xml 형태로 저장하므로 처리속도가 굉장히 느리다.
2. 그러므로 많은 항목을 다룰 땐 파일을 하나 만들어 사용하는 것이 좋다.
3. 보통 환경설정 저장용으로 사용된다.


**사용방법**

1. Preference Fragment 활용방법(Preference Activity 활용방법은 허니컴이후로 지원하지 않음.)



**사용방법 API**
정의하기(SharedPreferences) -> 불러오기(getSharedPreferences) -> 기록하기(editor)

[1] Preference 정의

[2] Preference 불러오기

[3] Preference 기록하기



프리퍼런스를 활용한 셋팅 창 만들기 예제

**소스코드**

1. 먼저 표시할 셋팅 Activity를 생성한다.
2. 매니페스트에서 메인 액티비티의 자식 액티비티로 설정한다.
3. 프리퍼런스 데이터 값을 사용하기 위해 리소스 패키지안에 xml 패키지를 만들고 PreferenceScreen 태그로 감싼 프리퍼런스 데이터 값을 만든다. 태그를 중첩하면 트리구조로 환경설정창을 만들 수 있다.

		패키지 경로 [res -> xml-> pref_exam.xml]
 	
		<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android">
	    <!-- 디폴트 벨류는 기본적인 Bass Circle을 보이게 한다. 키와 타이틀은 Show bass 설정-->
	    <!-- Summary는 이 체크박스가 어떤 역할을 하는지 유저에게 설명 hidden은 보이지 않게, shown은 보이게-->
	    <CheckBoxPreference
	        android:defaultValue="true"
	        android:key="show_bass"
	        android:summaryOff="Hidden"
	        android:summaryOn="Shown"
	        android:title="Show Bass"/>
	     </PreferenceScreen>		


4. 셋팅액비티에서 프래그먼트로 프리퍼런스를 표시할 것이므로 PreferenceFragmentCompat을 상속한 SettingFrament를 만든다.
	* onCreatePreference를 오버라이드하여 생성한 프리퍼런스 xml 파일을 addPreferenceFromResource로 가져와 형상화 한다.
5. 환경설정 창으로 넘어갈 때 오류가 안나게 하기 위해 style.xml에서 Theme을 추가해 주어야 한다.
	* ` <item name="preferenceTheme">@style/PreferenceThemeOverlay</item>`

6. 프래그먼트와 프리퍼런스를 연결하면 완성!.
 

---
### 2. 파일 활용

안드로이드에서 파일 저장 공간은 크게 두가지 이다.

1. 내부(Internal)
2. 외부(External)

이것은 안드로이드 초기부터 microSD카드를 확장할 수 있도록 내부와 외부SD카드 공간을 분리하였기 때문이다.

앱은 기본적으로 internal storage 에 저장되지만 manifest 파일내의 android:installLocation 속성으로 external storage 에 저장할 수 있다.

	예) apk 파일의 사이즈가 매우 큰경우



**내부저장공간**

내부저장공간은 앱에게 주어진 공간이므로 앱이 삭제될 때 데이터도 같이 사라진다. 그리고 다른앱에서 접근할 수 없다.

내부 저장공간은 두가지 디렉토리가 있다.

1. 파일저장공간
	* getFileDir()로 접근한다.
2. 캐쉬파일 저장공간 
	* 주기적으로 지워줘야함.
	* getCacheDir()로 접근한다.
3. 파일저장시 
	`File file = new File(getFileDir(), "testFileName");` 로 경로 설정한다.
4. 일반 파일을 내부공간에 저장할 때 - 파일 스트림으로 저장시 openFileOutput()이라는 메소드로 가져올 수 있습니다	
	* 인자로 저장할 파일이름과 저장모드를 넣어주어야 함. 저장모드는 현재 MODE_PRIVATE이다. 
5. 캐쉬 파일을 내부공간에 저장할 때 - 임시적인 이미지 저장할 때 주로사용
	* createTempFile()메소드를 사용하여 저장한다.
	* 예를 들어 카메라로 사진을 찍고 서버로 전송하기 위해 임시로 캐시폴더에 저장할때 사용.
	* createTempFile(String filename, String suffix(.jpg와같은), String uri(디렉토리 경로))
		* suffix가 null이면 자동으로 tmp를 붙여줌.




**외부저장공간**

유저가 외부SD카드를 빼버리거나 넣지 않을 경우, 존재하지 않는다.
이곳에 넣은 파일은 <u>어디서나 접근 가능</u>하다.

**외부 저장소가 연결되어 있으며 사용가능한지 여부 체크 - 메소드로 만들어서 리턴**

	public boolean isExternalStorageWritable(){
			String state = Environment.getExternalStorageState();
		if( Environment.MEDIA_MOUNTED.equals(state)){

			//MEDIA_MOUNTED 가 외부저장소(SDcard)에 읽기/쓰기 가능한지 판단하는 변수 
			return true;
		}
		return false;
	}
	
	public boolean isExternalStorageReadable(){
		String state = Environment.getExternalStorageState();
		if( Environment.MEDIA_MOUNTED.equals( state) ||
			Environment.MEDIA_MOUNTED_READ_ONLY.equals( state)){
			// MEDIA_MOUNTED_READ_ONLY 읽기만 가능한지 판단한다.
			return true;
		}
		return false;
	}
	
	

**external storage 공용 public 저장소 사용하기**

public 저장소는 앱을 삭제해도 저장되어 있는 파일이 사용가능하다.
	
	public File getAlbumStorageDir( String albumName){
		File file = new File( Environment.getExternalStoragePublicDirectory
		( Environment.DIRECTORY_PICTURES), 
			albumName
		);

		if( !file.mkdir()){
			Log.e( LOG_TAG, "Directory not created");
		}
		return file;
	}


**주요 변수**

	static File Environment.getExternalStoragePublicDirectory(String type)
	데이터 유형에 따른 외부 저장소의 저장 공간 경로를 반환합니다. 인자로 디렉터리의 유형을 넘겨줍니다.

**7가지 데이터 유형**

1. Environment.DIRECTORY_ALARMS 
	* 알람으로 사용할 오디오 파일을 저장합니다. --- /mnt/sdcard/Alarms 
2. Environment.DIRECTORY_DCIM 
	* 카메라로 촬영한 사진이 저장됩니다. --- /mnt/sdcard/DCIM  
3. Environment.DIRECTORY_DOWNLOADS 
	* 다운로드한 파일이 저장됩니다. --- /mnt/sdcard/Download 
4. Environment.DIRECTORY_MUSIC 
	* 음악 파일이 저장됩니다. --- /mnt/sdcard/Music  
5. Environment.DIRECTORY_MOVIES 
	* 영상 파일이 저장됩니다. --- /mnt/sdcard/Movies  
6. Environment.DIRECTORY_NOTIFICATIONS 
	* 알림음으로 사용할 오디오 파일을 저장합니다. --- /mnt/sdcard/Notifications  
7. Environment.DIRECTORY_PICTURES 
	* 그림 파일이 저장됩니다.  --- /mnt/sdcard/Pictures  
8. Environment.DIRECTORY_PODCASTS  
	* 팟캐스트(Poacast) 파일이 저장됩니다.  --- /mnt/sdcard/Podcasts


**external storage 개인 private 저장소 사용하기**

앱이 제거되면 private 저장소내의 모든 파일을 삭제한다.

	public File getAlbumStorageDir( String albumName){
		File file = new File( Environment.getExternalFilesDir( Environment.DIRECTORY_PICTURES), 
				albumName
		);
		// root 디렉토리 얻기
		// file = new File( Environment.getExternalFilesDir( null)); 
		
		if( !file.mkdir()){
			Log.e( LOG_TAG, "Directory not created");
		}
		return file;
	}

**주요 메소드**
	
	File Context.getExternalFilesDir(String type)
	애플리케이션 개인 private 영역의 데이터 유형에 따른 외부 저장소의 저장 공간 경로를 반환합니다. 
	인자로 디렉터리의 유형을 넘겨줍니다.

**저장공간 확인**

	getFreeSpace(), getTotalSpace()로 확인할 수 있다.


---

### 3.SQL lite 활용

복잡한 데이터를 이용하여 저장할 때 용이하다. 서버를 이용하지 않고 내부 저장소를 이용하여 데이터베이스를 이용하기 떄문에 단말내 데이터 처리에 용이하다.

공유저장소 - 외장 메모리

안드로이드는 파일시스템에 데이터를 저장하는데 2가지 방법 

1. SharedPreferences - 키-값 쌍 저장, Primitive 값(boolean, float, int, longs 등)
2. SQLite Database - 관계형 데이터 베이스, 테이블에 저장한다.(CRUD)

핵심 요소


안드로이드에서 SQLite 데이터베이스를 사용할 때 가장 중요한 핵심 요소는 SQLiteDatabase 클래스이다. SQLite 데이터베이스를 다루는 기능들이 SQLiteDatabase 클래스에 정의되어 있다.

데이터베이스의 핵심은 CRUD(Create, Read, Upload, Delete)

1. SQLiteDatabase.openOrCreateDatabase() 메소드로 SQLite 데이터베이스 파일이 존재하지 않으면 새로운 파일을 만들고 데이터베이스를 열어 사용한다. 존재하는 경우 이 메소드를 사용하여 파일을 연다.
2. 데이터베이스 테이블이 존재하지 않으면 Create 쿼리문을 execSQL(쿼리)메소드로 실행하여 테이블을 생성한다.
3. select 문으로 테이블의 한 row씩 데이터를 cursor로 가져와 뷰와 맵핑하여 데이터를 출력한다. 
4. 데이터 저장 - insert문으로
5. delete 문으로 테이블의 레코드 삭제로


**MainActivity.java 코드**
	 
	 SQLiteDatabase sqliteDB ;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
		sqliteDB = init_database() ;
        init_tables() ;		// 데이터베이스 파일이 처음인 경우 테이블 생성(Create)
        load_values() ;		// select문을 사용하여 데이터베이스를 조회한다.(Read)

        Button buttonSave = (Button) findViewById(R.id.buttonSave) ;
        buttonSave.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                save_values() ;     // 데이터 저장 - insert문으로 (Update)
            }
        });

        Button buttonClear = (Button) findViewById(R.id.buttonClear);
        buttonClear.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                delete_values();    // 데이터삭제 - (Delete)
            }
        });


    }

    private SQLiteDatabase init_database() {

        SQLiteDatabase db = null ;
        // File file = getDatabasePath("contact.db") ;
        File file = new File(getFilesDir(), "contact.db") ;

        System.out.println("PATH : " + file.toString()) ;
        try {
            db = SQLiteDatabase.openOrCreateDatabase(file, null) ;
        } catch (SQLiteException e) {
            e.printStackTrace() ;
        }

        if (db == null) {
            System.out.println("DB creation failed. " + file.getAbsolutePath()) ;
        }
        return db ;
    }

    // Create - 테이블을 처음 생성한다.
    private void init_tables() {
        if (sqliteDB != null) {
            String sqlCreateTbl = "CREATE TABLE IF NOT EXISTS CONTACT_T ("
                    + "NO " + "INTEGER NOT NULL,"
                    + "NAME "
                    + "TEXT,"
                    + "PHONE "
                    + "TEXT,"
                    + "OVER20 "
                    + "INTEGER" + ")";
            System.out.println(sqlCreateTbl);
            sqliteDB.execSQL(sqlCreateTbl);		// sql문을 실행하는 execSQL
        }
    }

    // sql select로 데이터를 가져온다.
    private void load_values() {

        if (sqliteDB != null) {
            String sqlQueryTbl = "SELECT * FROM CONTACT_T" ;
            Cursor cursor = null ;

            // 쿼리 실행
            cursor = sqliteDB.rawQuery(sqlQueryTbl, null) ;

            if (cursor.moveToNext()) { // 레코드가 존재한다면,
                // no (INTEGER) 값 가져오기.
                int no = cursor.getInt(0) ;
                EditText editTextNo = (EditText) findViewById(R.id.editTextNo) ;
                editTextNo.setText(Integer.toString(no)) ;

                // name (TEXT) 값 가져오기
                String name = cursor.getString(1) ;
                EditText editTextName = (EditText) findViewById(R.id.editTextName) ;
                editTextName.setText(name) ;

                // phone (TEXT) 값 가져오기
                String phone = cursor.getString(2) ;
                EditText editTextPhone = (EditText) findViewById(R.id.editTextPhone) ;
                editTextPhone.setText(phone) ;

                // over20 (INTEGER) 값 가져오기.
                int over20 = cursor.getInt(3) ;
                CheckBox checkBoxOver20 = (CheckBox) findViewById(R.id.checkBoxOver20) ;
                if (over20 == 0) {
                    checkBoxOver20.setChecked(false) ;
                } else {
                    checkBoxOver20.setChecked(true) ;
                }
            }
        }
    }
    // 데이터 저장 sql - 먼저 테이블 레코드를 모두 삭제한 후 Insert
    private void save_values() {
        if (sqliteDB != null) {

            // delete
            sqliteDB.execSQL("DELETE FROM CONTACT_T") ;

            EditText editTextNo = (EditText) findViewById(R.id.editTextNo) ;
            String noText = editTextNo.getText().toString() ;
            int no = 0 ;
            if (noText != null && !noText.isEmpty()) {
                no = Integer.parseInt(noText) ;
            }

            EditText editTextName = (EditText) findViewById(R.id.editTextName) ;
            String name = editTextName.getText().toString() ;

            EditText editTextPhone = (EditText) findViewById(R.id.editTextPhone) ;
            String phone = editTextPhone.getText().toString() ;

            CheckBox checkBoxOver20 = (CheckBox) findViewById(R.id.checkBoxOver20) ;
            boolean isOver20 = checkBoxOver20.isChecked() ;

            String sqlInsert = "INSERT INTO CONTACT_T " +
                    "(NO, NAME, PHONE, OVER20) VALUES (" +
                    Integer.toString(no) + "," +
                    "'" + name + "'," +
                    "'" + phone + "'," +
                    ((isOver20 == true) ? "1" : "0") + ")" ;

            System.out.println(sqlInsert) ;

            sqliteDB.execSQL(sqlInsert) ;
        }
    }

    // 데이터 삭제 sql문
    private void delete_values() {
        if (sqliteDB != null) {
            String sqlDelete = "DELETE FROM CONTACT_T" ;

            sqliteDB.execSQL(sqlDelete) ;

            EditText editTextNo = (EditText) findViewById(R.id.editTextNo) ;
            editTextNo.setText("") ;

            EditText editTextName = (EditText) findViewById(R.id.editTextName) ;
            editTextName.setText("") ;

            EditText editTextPhone = (EditText) findViewById(R.id.editTextPhone) ;
            editTextPhone.setText("") ;

            CheckBox checkBoxOver20 = (CheckBox) findViewById(R.id.checkBoxOver20) ;
            checkBoxOver20.setChecked(false) ;
        }
    }