# 使用手册----实现qq表情
Spannable span = SmileUtils
                        .getSmiledText(ctx, "[(D11)]");
                // 设置内容
                txtMsg.setText(span, TextView.BufferType.SPANNABLE);




//得到表情列表
 List<String> reslist = getExpressionRes(62);

 public List<String> getExpressionRes(int getSum) {
        List<String> reslist = new ArrayList<String>();
        for (int x = 0; x <= getSum; x++) {
            String filename = "f_static_0" + x;

            reslist.add(filename);

        }
        return reslist;

    }
    
    
   //初始化adapter
   ExpressionAdapter expressionAdapter = new ExpressionAdapter(this,
                1, reslist);
   gv_expression.setAdapter(expressionAdapter);
   gv_expression.setOnItemClickListener(this);

    @Override
    public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
        LogPriter.e("----------------" + position);
        String filename = expressionAdapter.getItem(position);
        try {

            if (!filename.equals("delete_expression")) { // 不是删除键，显示表情
                // 这里用的反射，所以混淆的时候不要混淆SmileUtils这个??
                Class clz = Class
                        .forName("com.kouclo.ochat.util.SmileUtils");
                Field field = clz.getField(filename);
                mEditTextContent.append(SmileUtils.getSmiledText(
                        ChatActivity.this, (String) field.get(null)));
            } else { // 删除文字或者表??
                LogPriter.e(mEditTextContent.getText().toString());
                if (!TextUtils.isEmpty(mEditTextContent.getText().toString())) {

                    int selectionStart = mEditTextContent
                            .getSelectionStart();// 获取光标的位??
                    if (selectionStart > 0) {
                        String body = mEditTextContent.getText()
                                .toString();
                        String tempStr = body.substring(0,
                                selectionStart);
                        int i = tempStr.lastIndexOf("[");// 获取最后一个表情的位置
                        if (i != -1) {
                            CharSequence cs = tempStr.substring(i,
                                    selectionStart);
                            if (SmileUtils.containsKey(cs
                                    .toString()))
                                mEditTextContent.getEditableText()
                                        .delete(i, selectionStart);
                            else
                                mEditTextContent.getEditableText()
                                        .delete(selectionStart - 1,
                                                selectionStart);
                        } else {
                            mEditTextContent.getEditableText()
                                    .delete(selectionStart - 1,
                                            selectionStart);
                        }
                    }
                }

            }
        } catch (Exception e) {
        }
    }


  //布局文件
    
    <GridView
        android:id="@+id/gv_expression"
        android:layout_width="match_parent"
        android:layout_height="150dp"
        android:background="@color/white"
        android:listSelector="@android:color/transparent"
        android:numColumns="9"
        android:scrollbars="none" />

