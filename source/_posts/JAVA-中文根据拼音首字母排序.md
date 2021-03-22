---
title: '[JAVA]-中文根据拼音首字母排序'
date: 2021-03-22 17:05:14
categories: JAVA
tags: java
---

**有些用户列表/品种列表等，常用A,B,C作为名称的搜索条件。**

依赖
```
<dependency>
    <groupId>com.belerweb</groupId>
    <artifactId>pinyin4j</artifactId>
    <version>2.5.1</version>
</dependency>
```

工具类
```
public class PinYinUtils {

    /**
     * 获取汉字对应的拼音
     *
     * @param chinese 汉字串
     * @return
     */
    public static String getFullSpell(String chinese) {
        StringBuffer bf = new StringBuffer();

        char[] arr = chinese.toCharArray();
        HanyuPinyinOutputFormat defaultFormat = new HanyuPinyinOutputFormat();
        defaultFormat.setCaseType(HanyuPinyinCaseType.UPPERCASE);
        defaultFormat.setToneType(HanyuPinyinToneType.WITHOUT_TONE);
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] > 128) {
                try {
                    bf.append(PinyinHelper.toHanyuPinyinStringArray(arr[i], defaultFormat)[0]);
                } catch (BadHanyuPinyinOutputFormatCombination e) {
                    e.printStackTrace();
                }
            } else {
                bf.append(arr[i]);
            }
        }

        return bf.toString();
    }
}
```

将查询的数据，重新进行处理
```
    /**
     * 封装根据拼音首字母的数据
     *
     * @param data
     * @return
     */
    private List<Map<String, Object>> getPinYinData(List<CpPetKind> data) {
        List<Map<String, Object>> list = new LinkedList<>();
        for (int i = 1; i <= 26; i++) {

            // 大写
            String big = String.valueOf((char) (96 + i)).toUpperCase();
            // 小写
            String little = big.toLowerCase();

            List<CpPetKind> initialsList = new LinkedList<>();
            for (int j = 0; j < data.size() - 1; j++) {
                CpPetKind petKind = data.get(j);

                String initials = PinYinUtils.getFullSpell(petKind.getBreed()).substring(0, 1);
                if (big.equals(initials) || little.equals(initials)) {
                    initialsList.add(petKind);
                }
            }

            Map<String, Object> tempMap = new HashMap<>();
            tempMap.put("initials", big);
            tempMap.put("kinds", initialsList);

            list.add(tempMap);
        }

        return list;
    }
```
