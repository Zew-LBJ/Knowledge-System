JDK安装:
解压包:tar -zxvf ...;
设置环境变量:vi /etc/profile
export JAVA_HOME=/opt/jdk1.8.0_211 //安装的具体目录
export PATH=$PATH:$JAVA_HOME/bin
环境配置生效
source /etc/profile
检查版本:
java -version