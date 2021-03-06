============================================================
==========  核心功能说明
============================================================
1. MC
nonObtuse有5种MC：Mid-point, interpolated, Nonobtuse-interpolated, Tilling Nonobtuse & Simple MC
目前Tilling Nonobtuse有运行时刻错误，其它都能运行通过，且都能产生完整的模型（如有缺失，是应为grid的分辨率不够）。
Mid-point & Simple MC 能生成无钝角的三角网格（Simple MC不都是对的）
interpolated, Nonobtuse-interpolated & Tilling Nonobtuse都生成含钝角的三角网格
前4种：Mid-point, interpolated, Nonobtuse-interpolated, Tilling Nonobtuse都是用了-1,1距离场；
最后一种直接进行三角片和grid的cell的求交。

蔡宇：可否将前4种和m_TriTable的对应说明一下？并把它们各自的pipeline画一下，如果雷同的话，标注一下即可。


2. -1,1距离场生成
我们得到的代买的距离场生成显然是错的，主要是因为在判断一个grid节点的内外（-1,1）的时候，没有考虑射线和网格相交的各种退化情况。（射线和三角片求交的各种情况考虑的是周全的）


3. deform to fit （optimization）
运行错误，王辉调试中


============================================================
==========  核心数据结构说明
============================================================
m_TriTable在3个class中：CIsoSurface，CNonobtuseMC，TilingNonobtuseMC
目前从网上得到的原始版本CIsoSurface的m_TriTable可以将所有的测试Model的SF生成完好的MC，但是钝角比较严重
现有的CIsoSurface，cnonobtusemc，tilingnonobtusemc的m_TriTable都不同程度地存在问题，cnonobtusemc的m_TriTable是目前SF生成MC，钝角最小也最少的
目前所有的m_TriTable已经放到了CIsoSurface.cpp中，可以通过宏#define作为开关，切换编译使用，对应关系如下：
USEISO：网上原始版本CIsoSurface的m_TriTable
USEIMC：CIsoSurface的m_TriTable
USENMC：cnonobtusemc的m_TriTable
USETMC：tilingnonobtusemc的m_TriTable
注意每次编译只能打开一个，注释掉其它！
对照文献JohnYungSanLiMSc.pdf的4.1中12c，可以在tilingnonobtusemc的m_TriTable中搜索到“-12, 5, 7, 11, 2, 1, 8, 0, 3”
在#ifdef USETMC下面，类型为12b，因此基本确定论文作者的实验是基于这个m_TriTable的
但是程序未必是论文的最终版本，因为tilingnonobtusemc中的CheckStiching函数直接导致程序生成MC时崩溃！
尝试屏蔽CheckStiching函数，生成网格的钝角问题也很严重！





============================================================
==========  核心函数说明
============================================================