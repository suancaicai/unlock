# unlock
Target的使用，手势解锁和指纹解锁此Demo主要技术点  有两个
一、多个Target开发项目

详细步骤请看【iOS多Target开发相似App . pdf】

二、指纹解锁和手势解锁

1、指纹使用系统方法
<LocalAuthentication/LocalAuthentication.h>

2、手势解锁   使用UIBezierPath 和CAShapeLayer 绘制图像

      a，  布局手势按钮，并且把所有的按钮存到数组（pointViewsArray）中
      b，  初始化开始点位和结束点位
      c,  touchesMoved 手势移动过程中保存pointView.ID到数组中，并改变按钮状态和划线
           c1.判断手势滑动是否在手势按钮范围
              c1-1.滑动到手势按钮范围，记录状态
                    c1-1-1如果开始按钮为zero，记录开始按钮，否则不需要记录开始按钮
                    c1-1-2判断该手势按钮的中心点是否记录，未记录则记录
                    c1-1-3判断该手势按钮是否已经选中，未选中就选中
           c2如果开始点位不为zero则记录结束点位，否则跳过不记录

      d,touchesEnded 手势结束将结束按钮设置为最后一个手势按钮的中心点，并画线
            d1如果endPoint还是为zero说明未滑动到有效位置，不做处理
            d2改变手势滑动结束的状态，为yes则无法在滑动手势划线
            d3存储绘制数据
            d4两次手势绘制的对比以及错误提示



问题   :存储的iD 与目前绘制的iD 使用isEqualToArray比较不成功， 就选择对比数组中的内容
看一下控制台输出：
Printing description of   selectedID:

            <__NSCFArray 0x6000002826c0>(
            gestures 1,
            gestures 2,
            gestures 3,
            gestures 6
            )
Printing description of  selectedViewArray:

            <__NSCFArray 0x600000282710>(
            gestures 1,
            gestures 2,
            gestures 3,
            gestures 6
            )



    - (BOOL)compareArray:(NSMutableArray *)arr1 andArr2:(NSMutableArray *)arr2 {
        bool bol = false;
        //判断数据是否相等
            if (arr2.count == arr1.count) {
                bol = true;
                //遍历比较数组数据的内容
                for (int16_t i = 0; i < arr1.count; i++) {
                    if (![[arr2 objectAtIndex:i] isEqualToString:[arr1 objectAtIndex:i]]) {
                        bol = false;
                        break;
                    }
                }
            }
        return bol? YES:NO;
    }


