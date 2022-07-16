## [Android] RecyclerView 사용법

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
## RecyclerView
ListView와 달리 RecyclerView는
1. ViewHolder를 무조건 구현해야 한다.
2. layoutmanager를 통해 가로·세로 리스트(linearlayoutmanager), 그리드 리스트(gridlayoutmanager), 각 크기가 다양한 그리드 리스트(staggeredlayoutmanager)를 만들 수 있다.
3. 아이템에 다양한 애니메이션을 줄 수 있다. (RecyclerView.ItemAnimator)
4. 아이템 디자인을 보다 다양한 부분으로 할 수 있지만 구현 또한 보다 복잡하다. (RecyclerView.ItemDecorator)
5. 아이템을 보다 잘 컨트롤 할 수 있지만 마찬가지로 코딩에서 복잡해졌다. (RecyclerView.OnItemTouchListener)
6. 리스트를 보여주는 퍼포먼스가 보다 빨라졌다.

\- [Kishore C S](https://medium.com/@kish.imss/listview-vs-recyclerview-2965d50b363)
# 리사이클러뷰 구성요소
- data class : 아이템에 반영할 데이터
- data list : 데이터를 담는 리스트(보통 ArrayList)
- item layout : 리스트 각 아이템의 디자인인 아이템 틀
- ViewHolder : 틀에 원하는 대로 데이터를 넣는 것
  <br>
  adapter 안에 위치
  <br>
  findViewById 메소드 처럼 xml과 데이터를 연결함.
- adapter : 데이터와 뷰홀더를 연결해주는 어댑터
- layoutManager : 리스트를 레이아웃에 보여주기 위한 것
- LayoutInflater : 아이템 레이아웃을 팽창시켜 아이템 객체 만듦
  <br>
  ViewHolder 안에 위치. 말 그대로 레이아웃을 팽창, 부풀리는 것.
  <br>
  한 아이템이 고유의 값(eg. id value)을 가지게 해 선택, 삭제 등의 작업을 용이하게 할 수 있도록 아이템 틀을 객체화함.

# 리사이클러뷰 만들기
## 만드는 순서
1. 액티비티 레이아웃 recyclerView 추가 (xml)
2. 아이템의 레이아웃 만들기 (xml)
3. 아이템의 데이터 클래스 만들기 (java/kotlin)
  <br>
  데이터를 넣을 변수 & 생성자 & getter,setter를 생성한다.
4. recyclerView의 adapter 만들기 (java/kotlin)
  <br>
  Adapter를 상속받은 CustomAdapter를 생성한다.
  <br>
  이때 반드시 생성되는 method가 있다.
  <br>
  - onCreateViewHolder : layoutInflater로 xml을 객체화하는 작업. ViewHolder를 생성해 return.
  <br>
  - onBindViewHolder : xml 내 위젯과 데이터를 묶는 작업. 각 item position에 해당하는 data를 ViewHolder의 item에 표시.
  <br>
  - getItemCount : 전체 데이터의 개수 return.
  <br>
  그 외 필요한 것들
  <br>
  - ViewHolder에 대한 (inner) class : xml의 위젯 선언과 id값 연결하는 작업. 위젯에 대한 이벤트 또한 제어. 
  <br>
  - Adapter constructor(생성자)
5. 액티비티에서 adapter와 layoutManager 연결하기(java/kotlin) 

## 코드
### Member 아이템 클래스
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
### CustomAdapter
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

### MainActivity
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