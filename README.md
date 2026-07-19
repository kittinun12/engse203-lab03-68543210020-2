## LAB 03 — Responsive Web UI & Form Interaction

  นาย กิตตินันท์ ผิวคำ 68543210020-2  

  คำอธิบาย - สร้างหน้าเว็บ responsive สำหรับระบบจัดการงานขนาดย่อม ฝึก semantic HTML, CSS layout, mobile-first design, event และ form validation.  



## ลิ้ง github page

https://kittinun12.github.io/engse203-lab03-68543210020-2/  

## ภาพประกอบ   

  <img width="1409" height="986" alt="Screenshot 2026-07-20 003357" src="https://github.com/user-attachments/assets/ba7b3701-ad38-4ffc-9eba-26f02c4297c4" />

  <img width="1204" height="725" alt="Screenshot 2026-07-20 004420" src="https://github.com/user-attachments/assets/9f99261d-0add-4177-83cd-f36db6bf4b3d" />



## วิธีติ้งตั้ง 

-npm run build

-npm run dev


## source code

  <img width="235" height="351" alt="Screenshot 2026-07-20 003656" src="https://github.com/user-attachments/assets/d3ae2599-46c1-4d9d-81d1-868ce42784dd" />


## main.js
  const form = document.querySelector('#profile-form');
const status = document.querySelector('#form-status');
const goalCount = document.querySelector('#goal-count');

const preview = {
  displayName: document.querySelector('#preview-name'),
  learningRole: document.querySelector('#preview-role'),
  learningGoal: document.querySelector('#preview-goal'),
};

function readForm() {
  return Object.fromEntries(new FormData(form).entries());
}

function renderPreview(data) {
  // TODO 6: อัปเดต preview ทั้ง 3 ค่าโดยใช้ textContent
  preview.displayName.textContent = data.displayName.trim() || 'ยังไม่ระบุชื่อ';
  preview.learningRole.textContent = data.learningRole || 'ยังไม่เลือกประเภท';
  preview.learningGoal.textContent = data.learningGoal.trim() || 'ยังไม่มีรายละเอียด';
  goalCount.textContent = `${data.learningGoal.length} ตัวอักษร`;
}

function validate(data) {
  // TODO 7: ตรวจชื่อ >= 2, role ต้องเลือก, goal >= 10
  const errors = {};

  if (data.displayName.trim().length < 2) {
    errors.displayName = 'กรุณากรอกชื่ออย่างน้อย 2 ตัวอักษร';
  }

  if (!data.learningRole) {
    errors.learningRole = 'กรุณาเลือกบทบาทที่สนใจ';
  }

  if (data.learningGoal.trim().length < 10) {
    errors.learningGoal = 'กรุณาเขียนเป้าหมายอย่างน้อย 10 ตัวอักษร';
  }

  return errors;
}

function renderErrors(errors) {
  // TODO 8: แสดง error ใกล้ field และกำหนด aria-invalid
  for (const name of ['displayName', 'learningRole', 'learningGoal']) {
    const field = form.elements[name];
    const output = document.querySelector(`#${name}-error`);
    const message = errors[name] ?? '';

    output.textContent = message;
    field.setAttribute('aria-invalid', String(Boolean(message)));
  }
}

// TODO 9: Read → Render
form.addEventListener('input', () => {
  const data = readForm();
  renderPreview(data);
});

form.addEventListener('submit', (event) => {
  event.preventDefault();
  // TODO 10: Read → Validate → Render errors/status

  const data = readForm();
  const errors = validate(data);
  renderErrors(errors);

  if (Object.keys(errors).length > 0) {
    renderStatus('invalid', 'ยังบันทึกไม่ได้ กรุณาตรวจสอบข้อมูล');
    form.querySelector('[aria-invalid="true"]')?.focus();
    return;
  }

  renderStatus('success', `พร้อมแล้ว ${data.displayName}! ข้อมูลผ่านการตรวจสอบ`);
});

function renderStatus(state, message) {
  status.dataset.state = state;
  status.textContent = message;
}

form.addEventListener('reset', () => {
  queueMicrotask(() => {
    // TODO 11: reset preview, errors และ status
    renderErrors({});
    renderPreview(readForm());
    renderStatus('idle', 'เริ่มพิมพ์เพื่อทดลอง Event และ Live Preview');
  });
});


