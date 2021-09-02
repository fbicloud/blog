# 解决Github pages百度收录问题

<!--more-->
由于某种不可抗拒力 导致***Github pages无法被百度收录*** 遂有了这篇文章
## 解决思路

{{< mermaid >}}
graph LR;
    A[访问] --> B{Dns判断}
    B --> C[用户]
    C --> D[Github pages]
    B --> E[蜘蛛]
    E --> F[腾讯对象存储]
{{< /mermaid >}}

​    

## Dnspod设置

### Github pages解析设置

{{< figure src="https://z3.ax1x.com/2021/08/31/hdcykt.png"  >}}

​    

### 对象存储解析设置

{{< figure src="https://z3.ax1x.com/2021/08/31/hdcI7n.png"  >}}
{{< admonition tip "This is a tip" open >}}
解析地址为对象存储的地址
{{< /admonition >}}

​    

## 使用Github Actions实现自动推送到腾讯对象存储

- {{< link "https://github.com/features/actions" "Github Actions主页" "去看看" >}}
- {{< link "https://cloud.tencent.com/document/product/436/10976#.E5.88.A0.E9.99.A4.E6.96.87.E4.BB.B6.E6.88.96.E6.96.87.E4.BB.B6.E5.A4.B9" "COSCMD 工具指南" "去看看" >}}

### Actions配置

```yaml
name: updates

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the main branch
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2


      - name: Run apt
        run: |
          sudo apt update -y
          sudo apt-get install python -y

      - name: Install comcmd
        run: |
        	pip install coscmd
        	
      - name: Updates
        run: | 
          coscmd config -a ${{ secrets.COS_ID }} -s ${{ secrets.COS_KEY }} -b ${{ secrets.COS_BUCKET_NAME }} -r ${{ secrets.COS_BUCKET_LOCATION }} -m 16 -p 10
          coscmd delete -r -f /
          coscmd upload -rs ${GITHUB_WORKSPACE}/ /
```


