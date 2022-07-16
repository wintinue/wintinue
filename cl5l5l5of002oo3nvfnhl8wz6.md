## [Android] RecyclerView 사용법

# 리사이클러뷰란?
## 정의
안드로이드 앱에서 다량의 데이터를 스크롤로 표시하기 위해 사용하는 위젯
> 앱에서 대량의 데이터 세트 또는 자주 변경되는 데이터에 기반한 요소의 스크롤 목록을 표시해야 한다면 이 페이지에서 설명하는 대로 RecyclerView 를 사용하면 됩니다.
<br>
\- [Android Developers﻿](https://developer.android.com/guide/topics/ui/layout/recyclerview?hl=ko)

## 등장 배경
한줄요약 | 기존 ListView의 문제 해결하고자 진보되고 유연한 RecyclerView 등장
### ListView의 문제점
같은 형식의 다량 데이터(각각을 아이템이라 함)를 보여주는 ListView가 있었다.
ListView는 아이템을 계속 생성 및 삭제하며 쭉 보여주는 방식으로, RAM의 메모리, 즉 리소스 사용률을 높이게 하였다.
물론 ViewHolder라는 것을 사용해 ListView의 리소스 사용률을 관리할 수 있었지만 강제가 아니었고, 이로 인해 ViewHolder를 사용해 리소스 사용률을 관리하는 경우는 많지 않았다.

안드로이드 초반에는 리소스 사용률이 높아도 앱의 규모가 작아 사용하는데에 문제가 없지만, 앱 서비스의 고도화로 점점 많은 데이터를 보여주는 앱이 많아지고 이로 인해 메모리를 많이 잡아먹는 경우가 많아지게 되었다. 결국 안드로이드에선 리소스를 관리하게 하는 ViewHolder를 반드시 구현하도록 하는 RecyclerView를 만들었다.
### ListView vs RecyclerView
RecyclerView는 ViewHolder를 강제하는 것 외에도 삭제 생성을 반복하는 ListView와 달리 화면에서 안 보이게 된 아이템의 틀을 새로 보여줄 아이템 틀로 재사용한다는 차이점이 있다.

||ListView|RecyclerView|
|-|-|-|
|아이템 표현 방식|화면에서 사라지고 나타날 때마다 삭제 및 생성 반복|화면에서 사라진 아이템의 틀을 새로 보여주는 아이템의 틀로 재사용|
|ViewHolder 구현여부|선택|필수|