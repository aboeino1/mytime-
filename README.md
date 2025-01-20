إنشاء برنامج كامل يتطلب كتابة الأكواد البرمجية اللازمة، وتحضير البنية التحتية لنظام يدعم الميزات المذكورة. سأساعدك بخطة كاملة وتفصيل الأكواد اللازمة، مع تحديد الخطوات التي تحتاج إليها لوضع المشروع قيد التنفيذ.


---

الخطة لتنفيذ المشروع:

1. إنشاء المشروع باستخدام Flutter:

Flutter هو إطار عمل متعدد المنصات يسمح بإنشاء تطبيقات لأنظمة Android وiOS.

2. الخلفية (Backend):

سيتم استخدام Node.js أو Python (Django) مع قاعدة بيانات مثل PostgreSQL أو Firebase لتخزين وإدارة البيانات.

3. الموقع الجغرافي:

استخدام Google Maps API لتحديد النطاق الجغرافي (2 متر).


4. إدارة المستخدمين والصلاحيات:

إنشاء معرف فريد لكل مستخدم.

استخدام نظام الأدوار الهرمي (مدير، مساعد مدير، مشرف، موظف).


5. البنية السحابية:

نشر البرنامج على خادم سحابي مثل AWS أو Google Cloud Platform.



---

هيكل الأكواد:

1. إنشاء الواجهة الأمامية (Frontend) باستخدام Flutter:

ملف main.dart لتشغيل التطبيق.

شاشات رئيسية: تسجيل الدخول، لوحة التحكم، الحضور، الإجازات.


مثال كود بسيط لشاشة تسجيل الدخول:

import 'package:flutter/material.dart';

class LoginScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              'تسجيل الدخول',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 20),
            TextField(
              decoration: InputDecoration(
                labelText: 'ID المستخدم',
                border: OutlineInputBorder(),
              ),
            ),
            SizedBox(height: 10),
            TextField(
              obscureText: true,
              decoration: InputDecoration(
                labelText: 'كلمة المرور',
                border: OutlineInputBorder(),
              ),
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                // Function to authenticate user
              },
              child: Text('تسجيل الدخول'),
            ),
          ],
        ),
      ),
    );
  }
}


---

2. إنشاء واجهة الخلفية (Backend) باستخدام Node.js:

إعداد خادم باستخدام Express.js.

إنشاء قواعد بيانات المستخدمين، الحضور، والإجازات.


مثال كود بسيط لإنشاء خادم Node.js:

const express = require('express');
const app = express();
const bodyParser = require('body-parser');
const { Pool } = require('pg');

// إعداد قاعدة البيانات
const pool = new Pool({
  user: 'your_db_user',
  host: 'localhost',
  database: 'attendance_system',
  password: 'your_db_password',
  port: 5432,
});

app.use(bodyParser.json());

// API لتسجيل الدخول
app.post('/login', async (req, res) => {
  const { userId, password } = req.body;
  const result = await pool.query(
    'SELECT * FROM users WHERE id = $1 AND password = $2',
    [userId, password]
  );
  if (result.rows.length > 0) {
    res.status(200).json({ message: 'Login successful', user: result.rows[0] });
  } else {
    res.status(401).json({ message: 'Invalid credentials' });
  }
});

// تشغيل الخادم
app.listen(3000, () => {
  console.log('Server is running on port 3000');
});


---

3. قاعدة البيانات:

استخدام PostgreSQL لتخزين البيانات.

مثال لإنشاء جداول:

CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    role VARCHAR(50),
    password VARCHAR(100),
    department_id INT
);

CREATE TABLE attendance (
    id SERIAL PRIMARY KEY,
    user_id INT,
    check_in TIMESTAMP,
    check_out TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

CREATE TABLE leaves (
    id SERIAL PRIMARY KEY,
    user_id INT,
    start_date DATE,
    end_date DATE,
    status VARCHAR(20),
    FOREIGN KEY (user_id) REFERENCES users(id)
);


---

4. التكامل مع Google Maps API:

استخدام Geolocation للتحقق من موقع المستخدم.

كود لتحديد النطاق الجغرافي:


app.post('/check-location', (req, res) => {
  const { latitude, longitude } = req.body;

  // موقع المكتب
  const officeLatitude = 30.123456;
  const officeLongitude = 31.123456;

  const distance = getDistanceFromLatLonInMeters(
    latitude,
    longitude,
    officeLatitude,
    officeLongitude
  );

  if (distance <= 2) {
    res.status(200).json({ message: 'Within range' });
  } else {
    res.status(400).json({ message: 'Out of range' });
  }
});

// دالة لحساب المسافة
function getDistanceFromLatLonInMeters(lat1, lon1, lat2, lon2) {
  const R = 6371e3; // نصف قطر الأرض بالمتر
  const φ1 = (lat1 * Math.PI) / 180;
  const φ2 = (lat2 * Math.PI) / 180;
  const Δφ = ((lat2 - lat1) * Math.PI) / 180;
  const Δλ = ((lon2 - lon1) * Math.PI) / 180;

  const a =
    Math.sin(Δφ / 2) * Math.sin(Δφ / 2) +
    Math.cos(φ1) * Math.cos(φ2) * Math.sin(Δλ / 2) * Math.sin(Δλ / 2);

  const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));

  return R * c; // المسافة بالمتر
}


---

تحميل الأكواد والمشروع:

يمكنني مساعدتك في رفع الأكواد إلى موقع مثل GitHub أو GitLab حيث يمكنك الوصول إليها مباشرة والبدء في تنفيذ المشروع. هل ترغب في ذلك؟

