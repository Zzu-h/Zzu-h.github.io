---
layout: post
title: "Android: RecyclerView"
date: 2022-04-27
excerpt: "Android RecyclerView"
tags: [UMC, RecyclerView, Android]
comments: true
categories : Android-기초
---

# RecyclerView
- 리스트 뷰와 같이 목록 화면을 구성한다.
- RecyclerView는 해당 클래스 단독으로는 목록을 만들 수 없다.
    - 다음의 구성요소를 활용하여 구현해야 함
        - ViewHolder
        - Adapter
        - LayoutManager
    - 위의 구성 요소는 아래 이미지의 역할들을 수행한다.
![RecyclerView](https://user-images.githubusercontent.com/72387349/165475154-6f23ca04-dbcf-45b6-a347-cfb913cd016a.jpeg)

## ViewHolder
- 뷰 홀더는 각 항목을 구성하는 뷰 객체를 가진다.
    - 목록에 있는 개별 항목의 레이아웃을 포함하는 View의 래퍼
- 현재 화면에 보이는 아이템 레이아웃 개수만큼 생성됨
- 새롭게 그려저야 할 아이템 레이아웃이 있다면(스크롤 동작) 가장 위의 ViewHolder를 재사용해서 데이터만 바꾼다

## Adapter
- 어댑터는 뷰 홀더에 있는 뷰 객체에 적절한 데이터를 대입해 각 아이템을 완성한다.
- RecyclerView의 Adapter는 RecyclerView.Adapter를 상속하며
    - 그 Adapter의 Type은 앞서 만들어둔 ViewHolder Class로 지정을 한다.
- 이 Adapter에 있는 데이터 리스트를 ViewHolder에 바인딩해주는 작업을 수행한다.
- 어댑터는 3개의 메소드를 오버라이딩을 해주어야 한다.
    - `override fun onCreateViewHolder(viewGroup: ViewGroup, viewType: Int): ViewHolder`
        - 항목의 뷰를 가지는 뷰 홀더를 준비하려고 자동으로 호출된다.
    - `override fun onBindViewHolder(viewHolder: ViewHolder, position: Int)`
        - 뷰 홀더의 뷰에 데이터를 출력하려고 자동으로 호출된다.
    - `override fun getItemCount(): Int`
        - 항목 개수를 판단하기 위해 자동으로 호출된다.

### ViewHolder 및 Adapter 사용법
```kotlin
class CustomAdapter(private val dataSet: Array<String>) :
        RecyclerView.Adapter<CustomAdapter.ViewHolder>() {

    /**
     * Provide a reference to the type of views that you are using
     * (custom ViewHolder).
     */
    class ViewHolder(view: View) : RecyclerView.ViewHolder(view) {
        val textView: TextView

        init {
            // Define click listener for the ViewHolder's View.
            textView = view.findViewById(R.id.textView)
        }
    }

    // Create new views (invoked by the layout manager)
    override fun onCreateViewHolder(viewGroup: ViewGroup, viewType: Int): ViewHolder {
        // Create a new view, which defines the UI of the list item
        val view = LayoutInflater.from(viewGroup.context)
                .inflate(R.layout.text_row_item, viewGroup, false)

        return ViewHolder(view)
    }

    // Replace the contents of a view (invoked by the layout manager)
    override fun onBindViewHolder(viewHolder: ViewHolder, position: Int) {

        // Get element from your dataset at this position and replace the
        // contents of the view with that element
        viewHolder.textView.text = dataSet[position]
    }

    // Return the size of your dataset (invoked by the layout manager)
    override fun getItemCount() = dataSet.size

}
```
## LayoutManager
- 위에서 어댑터가 만든 항목들을 어떻게 배치할 지를 결정한다.
- LayoutManager는 RecyclerView.LayoutManager를 상속받은 클래스이며 다음을 제공한다.
    - LinearLayoutManager
        - 항목을 가로나 세로 방향으로 배치
    - GridLayoutManager
        - 항목을 그리드로 배치
    - StaggeredGridLayoutManager
        - 항목을 높이가 불규칙한 그리드로 배치
- 또한 LayoutManager는 더 이상 화면에 표시되지 않는(no longer visible to the user) 아이템 뷰를 언제 재활용(recycle)할 것인지에 대한 정책도 결정하기도 한다.

## [ListView](./2022-04-27-ListView.md) vs RecyclerView
- ListView는 RecyclerView와 같이 항목을 구성할 때 사용되며, API Level 1부터 제공되었다.
- 이에 대한 차이를 알아보자

![listview-recyclerview](https://dev-to-uploads.s3.amazonaws.com/i/4h2wjwo0dlzkenw9ewnm.png)

- ListView는 스크롤 시 새로운 아이템을 계속해서 생성하고 과거의 오래된 아이템들은 삭제하는 과정을 수행한다.
    - 이는 리스트 개수가 많으면 많을수록 비효율적으로 동작하게 된다.
        - 하나의 아이템에 대해 최소 2번의 연산을 수행하게 됨
    - 하나의 아이템에 대해 틀은 같으나 데이터만 다를 뿐인 연산에 계속해서 객체를 삭제하고 생성하는 것은 비효율 적이다.
- RecyclerView는 스크롤 시 과거의 아이템을 삭제하지 않고 새로운 아이템이 나타날 위치로 배치 시킨다.
    - 이때 새로운 아이템의 데이터를 바인딩시키며 새로운 아이템인 것처럼 보이게 한다.
    - RecyclerView는 이와 같이 재사용을 하며 ListView보다 더욱 효율적인 구조를 가진다.

### 상세차이
- ListView와 RecyclerView의 동작차이는 위와 같다.
    - 다음으로 실제 필요한 요소, 제공되는 기능의 차이를 확인해보자.

||RecyclerView|ListView|
|:---:|:---|:---|
|ViewHolder|ViewHolder 패턴 이용이 필수적이다|ViewHolder를 이용할 필요가 없다|
|Adapter|데이터 제공을 위해 직접 구현해야 함|다양한 소스에 대한 어댑터가 존재함|
|Item Layout|가로, 세로, 불규칙적인 배치 모두 지원된다|세로만 지원됨|
|Item Animation|아이템 애니메이션 처리 클래스가 존재함|존재하지 않음|
|Decoration|많은 구분선 설정이 필요|자체적으로 구분선 기능이 있음|
|Click Event|개별 터치 이벤트 관리가 가능|클릭 이벤트에 바인딩하기 위한 인터페이스가 존재함|

- 전체적으로 사용하기에는 ListView가 구현하기가 더 수월하듯하다.
    - 하지만 사용자에게 보여지는 애니메이션, UI 측면에서는 RecyclerView가 훨씬 더 다양한 기능들을 제공하고 있다.
- 또한 성능적인 측면에서도 우월하므로 Android에서는 RecyclerView 사용을 권장하고 있다.

## DataList
- RecyclerView에 들어갈 데이터들의 목록
- ViewHolder에 이 리스트에서 꺼내온 데이터들을 집어넣으며 하나의 항목이 구성된다.
- 각 어댑터는 항목을 표시하기 위한 DataList가 필수적으로 필요하다.
- DataList에서 데이터가 변경되었을 때 RecyclerView는 이에 대한 감지를 인지하지 못 한다.
    - 따라서, 데이터들의 변경을 우리가 알려주어야 하는데, 그에 대한 메서드들은 다음과 같다.

### Notify Function

1. 전체 업데이트
    - notifyDataSetChanged
        - 리스트를 업데이트할 때 사용하는 메서드
        - 리스트의 크기와 아이템 모두 변경되는 경우에 사용
        - 모든 데이터를 새로 그리는 작업
            - 모든 상황 변화에 이용이 가능하나
            - 변화하는 환경에 맞게 Notify 함수를 적절히 사용하는 것이 좋다.
2. 변경
    - notifyItemChanged
        - 해당하는 위치의 데이터가 변경되었음을 알린다.
        - 이 메서드는 notifyItemRangeChanged를 호출하며 이때 크기가 1로써 사용된다.
    - notifyItemRangeChanged
        - 업데이트 하고 싶은 범위를 지정하여 모든 항목에 payload를 전달한다.
        - payload에 대한 [링크](https://zerogdev.blogspot.com/2018/07/recyclerview-notifyitemchanged-payload.html) 참고
3. 추가
    - notifyItemInserted
        - 특정 위치에 새로운 아이템이 삽입되었음을 알림
    - notifyItemRangeInserted
        - 특정 위치부터 n개의 아이템이 삽입되었음을 알림
4. 삭제
    - notifyItemRemoved
        - 특정 위치에 있는 아이템 1개를 삭제가 되었음을 알림
    - notifyItemRangeRemoved
        - 특정 위치부터 n개의 아이템이 삭제되었음을 알림
5. 이동
    - notifyItemMoved
        - 하나의 아이템 fromPosition 위치에서 toPosition으로 옮겨졌음을 알린다.
        - 아이템이 이동했을 때 알리는 메소드

## RecyclerView에서 Click Listener의 사용
- Android에서 Click Event를 처리하는 것은 크게 2가지 방법이 있다.
- 하나는 인터페이스를 활용, 하나는 람다식을 활용하는 것

### Interface를 활용
- 이는 Java에서도 많이 활용을 하고, 다양한 라이브러리에서 콜백 메서드로 활용된다.
- 이는 단순히 커스텀 인터페이스를 생성해서 해당 인터페이스를 통해 UI와 상호작용한다.
    - 또는 View.OnClickListener를 통해서 클릭위치를 받아서 사용해도 된다.
- 사용 방법은 다음과 같다.

```kotlin
class ProfileAdapter(val datas: mutableListOf<ProfileData>) : RecyclerView.Adapter<ProfileAdapter.ViewHolder>() {
    interface OnItemClickListener{
        fun onItemClick(v:View, data: ProfileData, pos : Int)
    }
    private var listener : OnItemClickListener? = null
    fun setOnItemClickListener(listener : OnItemClickListener) {
        this.listener = listener
    }
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder  = ViewHolder(parent.inflate(R.layout.item_recycler_ex))

     override fun getItemCount(): Int = datas.size

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.bind(datas[position])
    }

    inner class ViewHolder(val binding: ItemBinding) : RecyclerView.ViewHolder(view) {
        fun bind(item: ProfileData) {
            binding.txtName.text = item.name
            binding.txtAge.text = item.age.toString()
            Glide.with(itemView).load(item.img).into(binding.imgProfile)

            val pos = adapterPosition
            if(pos!= RecyclerView.NO_POSITION)
            {
                binding.root.setOnClickListener {
                    listener?.onItemClick(itemView,item,pos)
                }
            }
        }
    }
}
```

### 람다식 활용
- kotlin은 함수형 언어이다.
    - 함수형 언어의 장점 중 하나는 함수의 람다식이다.
- 이는 어댑에서 이름 없는 함수를 지어준다.
    - `val 변수명: 람다식 자료형 = { 람다식 }`
    - 그 후 우리가 원하는 동작에 대해 함수를 재정의 해준다.
- 사용 방법은 다음과 같다.
```kotlin
class AlbumRVAdapter(private val albumList: ArrayList<Album>) : RecyclerView.Adapter<AlbumRVAdapter.ViewHolder>(){
    var onItemClick:(Album) -> Unit = {}

    override fun onCreateViewHolder(viewGroup: ViewGroup, viewType: Int): ViewHolder
            = ViewHolder(ItemAlbumBinding.inflate(LayoutInflater.from(viewGroup.context), viewGroup, false))

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.bind(albumList[position])
        holder.itemView.setOnClickListener { onItemClick(albumList[position]) }
    }

    override fun getItemCount(): Int = albumList.size

    inner class ViewHolder(val binding: ItemAlbumBinding): RecyclerView.ViewHolder(binding.root){
        fun bind(album: Album){
            binding.itemAlbumTitleTv.text = album.title
            binding.itemAlbumSingerTv.text = album.singer
            binding.itemAlbumCoverImgIv.setImageResource(album.coverImg)
        }
    }
}
```

- 다음은 우리가 원하는 동작의 메소드를 등록한다.
    - albumItemClick은 실제 동작할 메소드이다.
```
binding.homeTodayMusicAlbumRv.adapter = AlbumRVAdapter(albumDatas)
    .apply { onItemClick = this@HomeFragment::albumItemClick }
```
