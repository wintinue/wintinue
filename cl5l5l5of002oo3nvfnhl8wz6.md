## [Android] RecyclerView 사용법, CRUD

# 리사이클러뷰란?
## 정의
안드로이드 앱에서 다량의 데이터를 스크롤로 표시하기 위해 사용하는 위젯
> 앱에서 대량의 데이터 세트 또는 자주 변경되는 데이터에 기반한 요소의 스크롤 목록을 표시해야 한다면 이 페이지에서 설명하는 대로 RecyclerView 를 사용하면 됩니다.
<br>
\- [Android Developers﻿](https://developer.android.com/guide/topics/ui/layout/recyclerview?hl=ko)

## 등장 배경
한줄요약 : 기존 ListView의 문제 해결하고자 진보되고 유연한 RecyclerView 등장
### ListView의 문제점
같은 형식의 다량 데이터(각각을 아이템이라 함)를 보여주는 ListView가 있었다.
ListView는 아이템을 계속 생성 및 삭제하며 쭉 보여주는 방식으로, RAM의 메모리, 즉 리소스 사용률을 높이게 하였다.
<br>
물론 ViewHolder라는 것을 사용해 ListView의 리소스 사용률을 관리할 수 있었지만 강제가 아니었고, 이로 인해 ViewHolder를 사용해 리소스 사용률을 관리하는 경우는 많지 않았다.

안드로이드 초반에는 리소스 사용률이 높아도 앱의 규모가 작아 사용하는데에 문제가 없지만, 앱 서비스의 고도화로 점점 많은 데이터를 보여주는 앱이 많아지고 이로 인해 메모리를 많이 잡아먹는 경우가 많아지게 되었다.
<br>
결국 안드로이드에선 리소스를 관리하게 하는 ViewHolder를 반드시 구현하도록 하는 RecyclerView를 만들었다.
### ListView vs RecyclerView
RecyclerView는 ViewHolder를 강제하는 것 외에도 삭제 생성을 반복하는 ListView와 달리 화면에서 안 보이게 된 아이템의 틀을 새로 보여줄 아이템 틀로 재사용한다는 차이점이 있다.
![recyclerview.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1657977996418/FvBrf4HHH.jpg align="center")

||ListView|RecyclerView|
|-|-|-|
|아이템 표현 방식|화면에서 사라지고 나타날 때마다 삭제 및 생성 반복|화면에서 사라진 아이템의 틀을 새로 보여주는 아이템의 틀로 재사용|
|ViewHolder 구현여부|선택|필수|
# 리사이클러뷰 기본 사용법
ListView와 달리 RecyclerView는
1. ViewHolder를 무조건 구현해야 한다.
2. layoutmanager를 통해 가로·세로 리스트(linearlayoutmanager), 그리드 리스트(gridlayoutmanager), 각 크기가 다양한 그리드 리스트(staggeredlayoutmanager)를 만들 수 있다.
3. 아이템에 다양한 애니메이션을 줄 수 있다. (RecyclerView.ItemAnimator)
4. 아이템 디자인을 보다 다양한 부분으로 할 수 있지만 구현 또한 보다 복잡하다. (RecyclerView.ItemDecorator)
5. 아이템을 보다 잘 컨트롤 할 수 있지만 마찬가지로 코딩에서 복잡해졌다. (RecyclerView.OnItemTouchListener)
6. 리스트를 보여주는 퍼포먼스가 보다 빨라졌다.

\- [Kishore C S](https://medium.com/@kish.imss/listview-vs-recyclerview-2965d50b363)
## 구성요소
- data class : 아이템에 반영할 데이터
- data list : 데이터를 담는 리스트(보통 ArrayList)
- item layout : 리스트 각 아이템의 디자인인 아이템 틀
- ViewHolder : 틀에 원하는 대로 데이터를 넣는 것
  adapter 안에 위치
  <br>
  findViewById 메소드 처럼 xml과 데이터를 연결함.
- adapter : 데이터와 뷰홀더를 연결해주는 어댑터
- layoutManager : 리스트를 레이아웃에 보여주기 위한 것
- LayoutInflater : 아이템 레이아웃을 팽창시켜 아이템 객체 만듦
  ViewHolder 안에 위치. 말 그대로 레이아웃을 팽창, 부풀리는 것.
  <br>
  한 아이템이 고유의 값(eg. id value)을 가지게 해 선택, 삭제 등의 작업을 용이하게 할 수 있도록 아이템 틀을 객체화함.

## 사용법
### 흐름
1. 액티비티 레이아웃 recyclerView 추가 (xml)
2. 아이템의 레이아웃 만들기 (xml)
3. 아이템의 데이터 클래스 만들기 (java/kotlin)
  데이터를 넣을 변수 & 생성자 & getter,setter를 생성한다.
4. recyclerView의 adapter 만들기 (java/kotlin)
  Adapter를 상속받은 CustomAdapter를 생성한다.

  이때 반드시 생성되는 method가 있다.
  <br>
  - onCreateViewHolder : layoutInflater로 xml을 객체화하는 작업. ViewHolder를 생성해 return.
  <br>
  - onBindViewHolder : xml 내 위젯과 데이터를 묶는 작업. 각 item position에 해당하는 data를 ViewHolder의 item에 표시.
  <br>
  - getItemCount : 전체 데이터의 개수 return.

  그 외 필요한 것들
  <br>
  - ViewHolder에 대한 (inner) class : xml의 위젯 선언과 id값 연결하는 작업. 위젯에 대한 이벤트 또한 제어. 
  <br>
  - Adapter constructor(생성자)
5. 액티비티에서 adapter와 layoutManager 연결하기(java/kotlin) 

### 코드
#### Member 아이템 클래스
```
public class Member {
    String name;
    int age;
    String job;

    public Member(String name, int age, String job) {
        this.name = name;
        this.age = age;
        this.job = job;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getJob() {
        return job;
    }

    public void setJob(String job) {
        this.job = job;
    }
}
```
#### CustomAdapter
```
public class CustomAdapter extends RecyclerView.Adapter<CustomAdapter.CustomViewHolder> {
    ArrayList<Member> members;
    Context mContext;

    public class CustomViewHolder extends RecyclerView.ViewHolder {
//        adapter의 viewHolder에 대한 inner class (setContent()와 비슷한 역할)
//        itemView를 저장하는 custom viewHolder 생성
//        findViewById & 각종 event 작업
        TextView tvName, tvAge, tvJob;

        public CustomViewHolder(@NonNull View itemView) {
            super(itemView);
            tvName = itemView.findViewById(R.id.item_name);
            tvAge = itemView.findViewById(R.id.item_age);
            tvJob = itemView.findViewById(R.id.item_job);
        }
    }

    public CustomAdapter(ArrayList<Member> members) {
//        adapter constructor
        this.members = members;
    }

    public CustomAdapter(ArrayList<Member> members, Context mContext) {
//        adapter constructor for needing context part
        this.members = members;
        this.mContext = mContext;
    }

    @NonNull
    @Override
    public CustomViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
//        onCreateViewHolder: make xml as an object using LayoutInflater & create viewHolder with the object
//        layoutInflater로 xml객체화. viewHolder 생성.
        View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.item_rv, parent, false);
        return new CustomViewHolder(view);
    }

    @Override
    public void onBindViewHolder(@NonNull CustomViewHolder holder, int position) {
//        onBindViewHolder: put data of item list into xml widgets
//        xml의 위젯과 데이터를 묶는(연결하는, setting하는) 작업.
//        position에 해당하는 data, viewHolder의 itemView에 표시함
        holder.tvName.setText(members.get(position).getName());
        holder.tvAge.setText(String.valueOf(members.get(position).getAge()));
        holder.tvJob.setText(members.get(position).getJob());
    }

    @Override
    public int getItemCount() {
//        getItemCount: return the size of the item list
//        item list의 전체 데이터 개수 return
        return (members != null ? members.size() : 0);
    }
}
```
line 48에 세번째 파라미터인 false를 하지 않으면 아래 에러가 떠서 빌드가 되지 않는다.
>java.lang.IllegalStateException: ViewHolder views must not be attached when created. Ensure that you are not passing 'true' to the **attachToRoot**

#### MainActivity
```
RecyclerView recyclerView;
ArrayList<Member> members = new ArrayList<>();
CustomAdapter adapter = new CustomAdapter(members, this);

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_basic);
        
    members.add(new Member("Kim", 12, "student"));
    members.add(new Member("Jake", 20, "programmer"));
    members.add(new Member("Tom", 41, "baker"));
    members.add(new Member("Conan", 32, "teacher"));
    members.add(new Member("Chris", 26, "COO"));
    members.add(new Member("John", 43, "PD"));
    members.add(new Member("Kate", 56, "professor"));
    members.add(new Member("Shara", 30, "student"));
    members.add(new Member("Megan", 14, "soccer player"));
    members.add(new Member("Josh", 17, "student"));
        
    recyclerView = findViewById(R.id.recyclerview);
    recyclerView.setLayoutManager(new LinearLayoutManager(this));
    recyclerView.setAdapter(adapter);
}
```
![recyclerview_simple.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657980202336/LAsR-9DQQ.png align="center")
# RecyclerView CRUD
리사이클러뷰 아이템을 추가, 수정, 삭제할 수 있다.

CRUD란 create(생성), read(조회), update(수정), delete(삭제)를 뜻한다. 철자 그대로 '씨알유디'라고 읽거나 '크루드'라고도 읽는다.
## Create 추가
### 동작
추가 버튼을 누르면 추가 액티비티로 이동한다.
<br>
추가 액티비티에서 정보 입력 후 확인 버튼을 누르면 메인 액티비티로 이동한다.
### 코드
#### activity_main
`추가` 버튼 추가하기
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerview"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1" />

    <Button
        android:id="@+id/btn_create"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:paddingTop="15dp"
        android:paddingBottom="15dp"
        android:text="@string/create" />
</LinearLayout>
```
#### activity_item
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:id="@+id/linearLayout"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        app:layout_constraintBottom_toTopOf="@+id/btn_submit"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintWidth_percent="0.6">

        <EditText
            android:id="@+id/edt_name"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="@string/name"
            android:inputType="text"
            android:textColor="@color/black"
            android:textSize="20sp" />

        <EditText
            android:id="@+id/edt_age"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="@string/age"
            android:inputType="numberDecimal"
            android:maxLength="3"
            android:textSize="20sp" />

        <EditText
            android:id="@+id/edt_job"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="@string/job"
            android:inputType="text"
            android:textSize="20sp" />
    </LinearLayout>

    <Button
        android:id="@+id/btn_submit"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:text="@string/submit"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintHeight_percent="0.12" />
</androidx.constraintlayout.widget.ConstraintLayout>
```
#### AddActivity
```
public class AddActivity extends AppCompatActivity {
    EditText edtName, edtAge, edtJob;
    Button btnSubmit;
    String name, job;
    int age;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_item);

        edtName = findViewById(R.id.edt_name);
        edtAge = findViewById(R.id.edt_age);
        edtJob = findViewById(R.id.edt_job);

        btnSubmit = findViewById(R.id.btn_submit);
        btnSubmit.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                name = edtName.getText().toString();
                job = edtJob.getText().toString();
                age = Integer.parseInt(edtAge.getText().toString());
                if (name.length() > 0 && job.length() > 0 && age > 0) {
                    Intent intent = new Intent(getApplicationContext(), MainActivity.class);
                    intent.putExtra("new", true);
                    intent.putExtra("name", name);
                    intent.putExtra("age", age);
                    intent.putExtra("job", job);
                    startActivity(intent);
                    finish();
                }
            }
        });
    }
}
```
#### MainActivity
버튼 클릭 리스너 추가
```
@Override
protected void onCreate(Bundle savedInstanceState) {
    btnCreate = findViewById(R.id.btn_create);
    btnCreate.setOnClickListener(new View.OnClickListener() {
      @Override
      public void onClick(View view) {
        startActivity(new Intent(getApplicationContext(), AddActivity.class));
      }
    });
}

@Override
    protected void onStart() {
        super.onStart();        
        String name, job;
        int age;
        name = getIntent().getStringExtra("name");
        job = getIntent().getStringExtra("job");
        age = getIntent().getIntExtra("age", -1);
        members.add(new Member(name, age, job));           
        }
    }
```
#### Manifest
MainActivity에 launchMode설정 (SharedPreferences 사용 안 하는 걸 전제로 하기 때문)
```
<activity
    android:name=".MainActivity"
    android:launchMode="singleTask">
      <intent-filter>
          <action android:name="android.intent.action.MAIN" />

          <category android:name="android.intent.category.LAUNCHER" />
      </intent-filter>
</activity>
```
### 결과
![c_item.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657981838752/Uw-XW5JXu.png align="center")
## Update 수정
### 동작
아이템을 클릭하면 정보를 수정 액티비티로 이동한다.
<br>
수정 액티비티에서 정보를 수정하고 확인을 누르면 메인 액티비티로 이동한다.
### 코드
#### CustomAdapter
어댑터에 아이템 클릭 이벤트 추가
 ```
public class CustomAdapter extends RecyclerView.Adapter<CustomAdapter.CustomViewHolder> {
    ArrayList<Member> members;
    Context mContext;

    public class CustomViewHolder extends RecyclerView.ViewHolder {
        //        adapter의 viewHolder에 대한 inner class (setContent()와 비슷한 역할)
        //        itemView를 저장하는 custom viewHolder 생성
//        findViewById & 각종 event 작업
        TextView tvName, tvAge, tvJob;

        public CustomViewHolder(@NonNull View itemView) {
            super(itemView);
//            item 에 대한 클릭 이벤트 설정
            tvName = itemView.findViewById(R.id.item_name);
            tvAge = itemView.findViewById(R.id.item_age);
            tvJob = itemView.findViewById(R.id.item_job);

            itemView.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View view) {
                    int position = getAdapterPosition();
                    if (position != RecyclerView.NO_POSITION) {
                        Intent intent = new Intent(mContext, EditActivity.class);
                        intent.putExtra("name", members.get(position).getName());
                        intent.putExtra("age", members.get(position).getAge());
                        intent.putExtra("job", members.get(position).getJob());
                        intent.putExtra("position", position);

                        mContext.startActivity(intent);
                    }
                }
            });            
        }
    }
```
#### EditActivity
```
public class EditActivity extends AppCompatActivity {
    EditText edtName, edtAge, edtJob;
    Button btnSubmit;
    String name, job;
    int age;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_item);

        edtName = findViewById(R.id.edt_name);
        edtAge = findViewById(R.id.edt_age);
        edtJob = findViewById(R.id.edt_job);

        name = getIntent().getStringExtra("name");
        job = getIntent().getStringExtra("job");
        age = getIntent().getIntExtra("age", -1);

        edtName.setText(name);
        edtAge.setText(String.valueOf(age));
        edtJob.setText(job);

        btnSubmit = findViewById(R.id.btn_submit);
        btnSubmit.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                name = edtName.getText().toString();
                job = edtJob.getText().toString();
                age = Integer.parseInt(edtAge.getText().toString());
                if (name.length() > 0 && job.length() > 0 && age > 0) {
                    Intent intent = new Intent(getApplicationContext(), MainActivity.class);
                    intent.putExtra("edit", true);
                    intent.putExtra("name", name);
                    intent.putExtra("age", age);
                    intent.putExtra("job", job);
                    intent.putExtra("position", getIntent().getIntExtra("position", -1));
                    startActivity(intent);
                    finish();
                }
            }
        });
    }
}
```
#### MainActivity
데이터 수정 시 아이템 정보 수정하는 코드 추가
```
 @Override
 protected void onNewIntent(Intent intent) {
     super.onNewIntent(intent);
     setIntent(intent);
 }

@Override
protected void onStart() {
    super.onStart();
    int action = 0;
    if (getIntent().getBooleanExtra("new", false)) action = 1;
    else if (getIntent().getBooleanExtra("edit", false)) action = 2;
    Log.d(TAG, "onStart: " + action);
    if (action > 0) {
        String name, job;
        int age;
        name = getIntent().getStringExtra("name");
        job = getIntent().getStringExtra("job");
        age = getIntent().getIntExtra("age", -1);
        if (action == 1) members.add(new Member(name, age, job));
        else { // when action == 2
            int position = getIntent().getIntExtra("position", -1);
            if (position != -1) {
                members.get(position).setName(name);
                members.get(position).setAge(age);
                members.get(position).setJob(job);
            }
        }
        adapter.notifyDataSetChanged();
    }
}
```
### 결과
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657982213206/zA7gp_TtQ.png align="center")
## Delete 삭제
### 동작
아이템 롱클릭 시 삭제하겠냐는 다이얼로그가 나타난다.
<br>
삭제하기를 선택하면 해당 아이템이 삭제된다.
### 코드
#### CustomViewHolder
```
public class CustomViewHolder extends RecyclerView.ViewHolder {

		...

public CustomViewHolder(@NonNull View itemView) {
    super(itemView);

    ...

    itemView.setOnLongClickListener(new View.OnLongClickListener() {
        @Override
        public boolean onLongClick(View view) {
            int position = getAdapterPosition();
            if (position != RecyclerView.NO_POSITION) {
                AlertDialog.Builder builder = new AlertDialog.Builder(mContext);
                builder.setTitle("삭제하기")
                        .setMessage(members.get(position).getName() + "을(를) 삭제하시겠습니까?")
                        .setPositiveButton("삭제하기", new DialogInterface.OnClickListener() {
                            @Override
                            public void onClick(DialogInterface dialogInterface, int i) {
                                members.remove(position);
                                notifyDataSetChanged();
                            }
                        })
                        .setNeutralButton("취소", null)
                        .show();
                }
                return false;
            }
        });
    }
}
```
### 결과
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657982856236/OFdeeVzST.png align="center")
