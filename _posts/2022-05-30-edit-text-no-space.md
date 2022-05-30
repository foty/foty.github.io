---
layout: post
title:  "EditTxt禁止输入空格方法"
date:   2022-05-30 11:54:30 +0800
categories: articles
tags: [problem]
---
...

### 第一种方式：直接修改监听器逻辑，不允许输入空格：
```text
TextWatcher textChanged = new TextWatcher() {
    @Override
    public void beforeTextChanged(CharSequence charSequence, int i, int i1, int i2) { }

    @Override
    public void onTextChanged(CharSequence charSequence, int start, int before, int count) {
        // 禁止EditText输入空格
        if (charSequence.toString().contains(" ")) {
            String[] str = charSequence.toString().split(" ");
            StringBuffer sb = new StringBuffer();
            for (int i = 0; i < str.length; i++) {
                sb.append(str[i]);
            }
            editText.setText(sb.toString());
            editText.setSelection(start);
        }
    }

    @Override
    public void afterTextChanged(Editable editable) { }
};
editText.addTextChangedListener(textChanged);
```


###  第二种：通过设置InputFilter
```text
 InputFilter filter = new InputFilter() {
     @Override
     public CharSequence filter(CharSequence source, int start, int end, Spanned dest, int dstart, int dend) {
         if (source.equals(" "))
          return "";
         else
          return null;
     }
 };
 editText.setFilters(new InputFilter[]{filter});
```