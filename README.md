#若水新闻客户端，修改适配过的代码供自己参考 
	CustomSimpleAdapter categoryAdapter = new CustomSimpleAdapter(this, categories, R.layout.category_title, new String[]{"category_title"}, new int[]{R.id.category_title});
		
		GridView category = new GridView(this);
		category.setColumnWidth(mColumnWidthDip);//每个单元格宽度
		category.setNumColumns(GridView.AUTO_FIT);//单元格数目
		category.setGravity(Gravity.CENTER);//设置对其方式
		//设置单元格选择是背景色为透明，这样选择时就不现实黄色北京
		category.setSelector(new ColorDrawable(Color.TRANSPARENT));
		//根据单元格宽度和数目计算总宽度
		int width = mColumnWidthDip * categories.size();
		LayoutParams params = new LayoutParams(width, LayoutParams.FILL_PARENT);
		//更新category宽度和高度，这样category在一行显示
		category.setLayoutParams(params);
		//设置适配器
		category.setAdapter(categoryAdapter);
		//把category加入到容器中
		LinearLayout categoryList = (LinearLayout) findViewById(R.id.category_layout);
		categoryList.addView(category);
		//添加单元格点击事件
		category.setOnItemClickListener(new OnItemClickListener()
		{
			@Override
			public void onItemClick(AdapterView<?> parent, View view, int position, long id)
			{
				TextView categoryTitle;
				//恢复每个单元格背景色
				for (int i = 0; i < parent.getChildCount(); i++)
				{
					categoryTitle = (TextView) (parent.getChildAt(i));
					categoryTitle.setBackgroundDrawable(null);
					categoryTitle.setTextColor(0XFFADB2AD);
				}
				//设置选择单元格的背景色
				categoryTitle = (TextView) (parent.getChildAt(position));
				categoryTitle.setBackgroundResource(R.drawable.categorybar_item_background);
				categoryTitle.setTextColor(0XFFFFFFFF);
				//获取选中的新闻分类id
				mCid = categories.get(position).get("category_title").getCid();
				mCatName = categories.get(position).get("category_title").getTitle();
				Log.v(loginf, "-------mcid="+mCid+"mCatName="+mCatName);
				//获取该栏目下新闻
				//getSpeCateNews(mCid,mNewsData,0,true);
				//通知ListView进行更新
				//mNewsListAdapter.notifyDataSetChanged();
				loadNewsAsyncTask = new LoadNewsAsyncTask();
				loadNewsAsyncTask.execute(mCid,0,true);
			}
		});
