# meeting-room-sync
本网页由AI生成
实现效果：
电脑、手机端访问网页，选定会议室并复制会议链接推送到会议室端，会议室端网页收到推送链接后拉起TEAMS客户端
例如电脑端访问https://meeting-room-sync.vercel.app
<img width="1850" height="1617" alt="2026-03-20_161834" src="https://github.com/user-attachments/assets/52182481-a25b-4918-a769-1451af275c7f" />

会议室端网页端需要加上参数，例如会议2， /?room=room2
https://meeting-room-sync.vercel.app/?room=room2


如需修改会议室，请将index中会议室号注释去掉。



数据库端操作
第一步：注册 Supabase
1. 打开浏览器访问
https://supabase.com/

2. 创建项目
点击 "New Project"
填写信息：
Name: meeting-room
Database Password: Meeting@2026  （记住这个密码）
Region: Southeast Asia (Singapore)  ⚠️ 重要：选新加坡
点击 "Create new project"

3. 创建数据表
项目创建好后：
左侧菜单找到 "SQL Editor"（看起来像 </> 图标）
点击 "New query"
复制粘贴以下代码：

Sql

-- 创建会议表
CREATE TABLE meetings (
  room_id TEXT PRIMARY KEY,
  link TEXT NOT NULL,
  title TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

-- 启用实时功能
ALTER TABLE meetings REPLICA IDENTITY FULL;

-- 设置权限（允许所有人读写）
ALTER TABLE meetings ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Allow all operations" 
ON meetings 
FOR ALL 
USING (true) 
WITH CHECK (true);
点击右下角 "Run" 按钮

看到 ✅ "Success. No rows returned" 就成功了

4. 获取 API 密钥
左侧菜单点击 "Settings"（齿轮图标）

选择 "API"

复制以下两个值（先粘贴到记事本保存）：

Project URL: https://xxxxx.supabase.co
anon public key: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.ey...（很长一串）
✅ Supabase 配置完成！
