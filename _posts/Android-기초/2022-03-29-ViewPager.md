---
layout: post
title: "Android: ViewPager"
date: 2022-03-29
excerpt: "Android ViewPager"
tags: [UMC, ViewPager, Android]
comments: true
categories : Android-기초
---

# ViewPager
- 스와이프할 수 있는 형식으로 뷰 또는 프래그먼트를 표시
- Dependency 추가
    - `implementation ‘androidx.viewpager2:viewpager2:${VERSION}’`
- 기존에 ViewPager를 지원하고 있었지만 현재 더 다양한 기능을 지원하는 ViewPager2가 나왔다.
    - Android 공식문서에서 ViewPager2를 사용하기를 권장하고 있다.
    - ViewPager2는 RecyclerView 컴포넌트를 기반으로 만들어졌기에 기존 ViewPager보다 Cost가 절감된다.

## Adapter
- ViewPager2는 RecycerView를 기반으로 만들어졌기에 RcyclerView.Adapter를 사용할 수도 있다.
    - 또한 FragmentStateAdapter를 사용할 수 있다.
- 종류
    - RcyclerView.Adapter
    - FragmentStateAdapter

### RcyclerView.Adapter
- RecyclerView에서 사용하는 Adapter와 크게 다를 것이 없다.
- Adapter 예시
    ```kotlin
    class MyPagerViewHolder(val binding: ItemPagerBinding) : 
        RecyclerView.ViewHolder(binding.root)
    class MyPagerAdapter(val datas: MutableList<String>) : 

        RecyclerView.Adapter<RecyclerView.ViewHolder>() {
            override fun getItemCount(): Int = datas.size

        override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): RecyclerView.ViewHolder 
            = MyPagerViewHolder(ItemPagerBinding.inflate(LayoutInflater.from(parent.context), parent, false))

        override fun onBindViewHolder(holder: RecyclerView.ViewHolder, position: Int) {
            val binding = (holder as MyPagerViewHolder).binding
            // 뷰에 데이터 출력
            binding.itemPagerTextView.text = datas[position]
            when (position % 3) {
                0 -> binding.itemPagerTextView.setBackgroundColor(Color.RED)
                1 -> binding.itemPagerTextView.setBackgroundColor(Color.BLUE)
                2 -> binding.itemPagerTextView.setBackgroundColor(Color.GREEN)
            }
        }
    }
    ```
- ViewPager Adapter 적용
    ```kotlin
    binding.viewpager.adapter = MyPagerAdapter(datas)
    ```

### 🎯 FragmentStateAdapter
- 대부분 화면은 복잡하게 작성된다.
    - 따라서, 일반적으로 프래그먼트로 작성하게 되는데 항목을 Fragment로 작성했다면
    - FragmentStateAdapter로 ViewPager2를 구현한다.
    - 이는 Fragment의 lifecycle을 관리해주기 위함이다.
- Adapter 예시
    ```kotlin
    class MyFragmentPagerAdapter(activity: FragmentActivity): FragmentStateAdapter(activity) {
        val fragments: List<Fragment>
        init {
            fragments= listOf(OneFragment(), TwoFragment(), ThreeFragment())
            Log.d(“kkang” ,”fragments size : ${fragments.size}”)
        }
        override fun getItemCount(): Int = fragments.size
        override fun createFragment(position: Int): Fragment = fragments[position]
    }
    ```
- ViewPager Adapter 적용
    ```kotlin
    binding.viewpager.adapter = MyPagerAdapter(datas)
    ```

## TabLayout과 ViewPager
- 종종 화면을 넘기면서 상하단에 있는 TabLayout의 초점도 같이 해당 Fragment로 옮겨가는 것을 확인할 수 있다.
- 우리는 다음과 같이 선언해주며 연동할 수 있다.
    ```kotlin
    TabLayoutMediator(tabLayout, viewPager) { tab, position ->
        tab.text = "Tab ${position+1}"
    }.attach()
    ```
> **TabLayoutMediator?** <br/>
> TabLayout와 ViewPager2를 연결하는 mediator이다. <br/>
> mediator는 필수적으로 tabLayout과 viewPager, 탭의 텍스트를 설정하는 TabConfigurationStrategy Interface를 담아주어야 한다. <br/>

## 🎯 Indicator
![viewpager-indicator](https://raw.githubusercontent.com/ongakuer/CircleIndicator/master/screenshot.gif)
- ViewPager를 이용한 화면이 현재 몇 번째 화면인가를 표시하기 위한 뷰
- 찾아본 결과 총 3가지 방식으로 구현할 수 있다.(추가적인 방식이 있다면 댓글을ㅎ)
    - 외부 라이브러리를 사용
    - Indicator를 구현
        - View를 상속받는 새로운 class를 만들어서 사용
    - TabLayout을 이용하여 구현

### 외부 라이브러리를 사용
- 이는 어렵지 않으므로 참고 사이트 (링크)[https://jaehoney.tistory.com/17]를 개재한다.

### Indicator를 구현
- View를 상속받는 새로운 class를 만들어서 사용
- custom CircleIndicator class
    ```kotlin
    class CircleIndicator: LinearLayout {
        private var mContext: Context? = null
        private var mDefaultCircle: Int = 0
        private var mSelectCircle: Int = 0
        private var _itemSize: Float = 0f
        private var _marginLeftAndRight: Float = 0f

        var itemSize: Float get() = _itemSize
            set(value) {
                _itemSize = TypedValue.applyDimension(
                    TypedValue.COMPLEX_UNIT_DIP, value.toFloat(), resources.displayMetrics)
            }
        var marginLeftAndRight: Float get() = _marginLeftAndRight
            set(value) {
                _marginLeftAndRight = TypedValue.applyDimension(
                    TypedValue.COMPLEX_UNIT_DIP, value.toFloat(), resources.displayMetrics)
            }

        private var item: MutableList<View> = mutableListOf()

        constructor(context: Context) : super(context) { mContext = context }
        constructor(context: Context, attrs: AttributeSet) : super(context, attrs) { mContext = context }

        fun setViewPager(viewPager: ViewPager2, defaultCircle: Int, selectCircle: Int, itemSize: Int = 10, marginLeftAndRight: Int = 5){
            this.itemSize = itemSize.toFloat()
            this.marginLeftAndRight = marginLeftAndRight.toFloat()

            createDotPanel(viewPager.adapter!!.itemCount, defaultCircle, selectCircle, 0)

            viewPager.registerOnPageChangeCallback(object: ViewPager2.OnPageChangeCallback(){
                override fun onPageSelected(position: Int) {
                    super.onPageSelected(position)
                    Log.d("pageSelected",position.toString())
                    selectDot(position)
                }
            })
        }

        private fun createDotPanel(count: Int, defaultCircle: Int, selectCircle: Int, position: Int) {
            this.removeAllViews()
            mDefaultCircle = defaultCircle
            mSelectCircle = selectCircle

            val selectDrawable: Drawable? = ContextCompat.getDrawable(context, mSelectCircle)
            val unSelectDrawable: Drawable? = ContextCompat.getDrawable(context, mDefaultCircle)

            for (i in 0 until count) {
                item.add(View(mContext).apply {
                    layoutParams = LayoutParams((itemSize+marginLeftAndRight).toInt(), itemSize.toInt())
                    background = if(i == position) selectDrawable
                    else unSelectDrawable
                })
                this.addView(item[i])
                println("$i")
            }
            this.gravity = Gravity.CENTER
        }

        /**
        * 선택된 점 표시
        * @param position
        */
        private fun selectDot(position: Int) {
            val selectDrawable: Drawable? = ContextCompat.getDrawable(context, mSelectCircle)
            val unSelectDrawable: Drawable? = ContextCompat.getDrawable(context, mDefaultCircle)

            println(selectDrawable.toString())
            for (i in item.indices)
                item[i].background = if (i == position) selectDrawable else unSelectDrawable
        }
    }
    ```
#### 실제 사용
- xml에 구현한 CicleIndicator를 선언한다.
    - 그 후 선택되었을 때/기본 background Drawable을 구현하고
    - code상에서 모든 값들을 연결해 준다.
- xml
    ```xml
    <androidx.viewpager2.widget.ViewPager2
        android:id="@+id/home_panel_background_vp"
        android:layout_width="match_parent"
        android:layout_height="430dp"
        android:scaleType="centerCrop"
        android:src="@drawable/img_first_album_default"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
    <com.example.indicator.CircleIndicator
        android:id="@+id/home_panel_viewpager_ci"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="8dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/home_panel_background_vp"/>
    ```
- ViewPage와 연결
    ```kotlin
    binding.homePanelViewpagerCi.setViewPager(binding.homePanelBackgroundVp, R.drawable.default_dot, R.drawable.selected_dot)
    ```

### TabLayout을 이용하여 구현
- 이는 TabLayoutMediator를 이용하면 간단하다.
    - 바로 예제를 통해서 알아보자
- xml
    ```xml
    <androidx.viewpager2.widget.ViewPager2
        android:id="@+id/home_panel_background_vp"
        android:layout_width="match_parent"
        android:layout_height="430dp"
        android:scaleType="centerCrop"
        android:src="@drawable/img_first_album_default"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
    <com.google.android.material.tabs.TabLayout
        android:id="@+id/home_panel_viewpager_tl"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:tabBackground="@drawable/view_pager_indicator_selector"
        app:tabGravity="center"
        app:tabIndicatorHeight="0dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/home_panel_background_vp"/>
    ```
- ViewPage와 연결
    ```kotlin
    TabLayoutMediator(binding.homePanelViewpagerTl, binding.homePanelBackgroundVp){ tab, position -> tab.text }
                .attach()
    ```
