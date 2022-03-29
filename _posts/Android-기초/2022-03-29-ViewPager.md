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

### FragmentStateAdapter
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