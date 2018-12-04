---
title: genCascaderOptionType工具函数
date: 2018-11-29 18:14:56
categories: antd
tags: antd,react
---
 最近接到一个需求，需要把后端给到的数据转换成antd里面树状选择器，为了方便下次使用就洗了一个工具函数：
 ```
 function isGeoNode(info: GeoNode | CategoryTreeRsp): info is GeoNode {
     return (info as GeoNode).zipCode !== undefined;

 }
 // 递归生成CascaderOptionType
export const genCascaderOptionType = (info: GeoNode | CategoryTreeRsp): CascaderOptionType => {
    const type: CascaderOptionType = {
            label: info.label,
                    value: info.key,
                            zipcode: isGeoNode(info) ? info.zipCode : ""
                                
    };
        const subChildMenus = info.children;
            let children = [];
            if (subChildMenus && subChildMenus.length > 0) {
                for (const sc of subChildMenus) {
                            children.push(genCascaderOptionType(sc));
                                    
                }
                    
            }
                type.children = children;
                    return type;

};
 ```
- 因为之前没有接触过Typescript,所以基本上都是在公司现学现卖，做到这个地方的时候因为这个城市的ZipCode不是一定存在的，这就需要你把ZipCode设置为
可选值，因为之前做的类型都是一定存在的，所以当时还挺坑，最后在他们官网找到了这个地方，所以想在这里记下来，一个方便自己下次查阅，另一个也可以
给下次可能用到这个的小哥哥姐姐们一个帮助~
