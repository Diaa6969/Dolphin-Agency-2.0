const spinButton = document.getElementById('spinButton');
const nameInput = document.getElementById('nameInput');
const addButton = document.getElementById('addButton');
const nameList = document.getElementById('nameList');
const resetButton = document.getElementById('resetButton');
const setLastTenButton = document.getElementById('setLastTenButton');
const orderSelect = document.getElementById('orderSelect');

let allNames = [];  // قائمة تحتوي على جميع الأسماء المدخلة
let lastTenNames = [];  // آخر 10 أسماء سيتم سحبها
let otherNames = [];  // الأسماء التي سيتم سحبها قبل الأسماء العشرة الأخيرة

// إضافة اسم جديد
addButton.addEventListener('click', () => {
    const name = nameInput.value.trim();
    if (name) {
        allNames.push(name);  // إضافة الاسم إلى القائمة الكبيرة

        // تحديث القائمة
        updateList();
        nameInput.value = '';  // إعادة تعيين حقل الإدخال
    }
});

// تحديد آخر عشرة أسماء
setLastTenButton.addEventListener('click', () => {
    if (allNames.length > 10) {
        const selectedNames = allNames.slice(-10);  // أخذ آخر 10 أسماء من القائمة
        lastTenNames = selectedNames;
        otherNames = allNames.slice(0, allNames.length - 10);  // الأسماء التي سيتم سحبها أولًا
        alert('تم تحديد آخر عشرة أسماء للاختيار منهم');
        updateList();  // تحديث القائمة بعد تحديد الأسماء
    } else {
        alert('يجب أن يكون لديك أكثر من 10 أسماء في القائمة');
    }
});

// تحديث القائمة لعرض الأسماء المدخلة
function updateList() {
    nameList.innerHTML = '';  // إعادة تعيين القائمة
    allNames.forEach(name => {
        const li = document.createElement('li');
        li.textContent = name;
        nameList.appendChild(li);
    });
}

// تدوير العجلة واختيار اسم عشوائي
spinButton.addEventListener('click', () => {
    if (otherNames.length > 0 || lastTenNames.length > 0) {
        let order = orderSelect.value;  // الحصول على ترتيب السحب من القائمة المنسدلة (من 10 إلى 1 أو العكس)

        // دمج الأسماء التي لم يتم سحبها مع الأسماء العشرة الأخيرة وفقًا للترتيب الذي اخترته
        let finalList = [...otherNames];
        if (order === "desc") {
            lastTenNames.reverse();  // عكس ترتيب الأسماء العشرة الأخيرة إذا كان من 10 إلى 1
        }

        finalList = finalList.concat(lastTenNames);  // إضافة الأسماء العشرة الأخيرة إلى النهاية

        // اختيار اسم عشوائي
        const randomIndex = Math.floor(Math.random() * finalList.length);
        const selectedName = finalList[randomIndex];
        alert('الاسم المختار: ' + selectedName);

        // إزالة الاسم المختار من القائمة
        if (finalList === otherNames) {
            otherNames = otherNames.filter(name => name !== selectedName);
        } else {
            lastTenNames = lastTenNames.filter(name => name !== selectedName);
        }

        updateList();  // تحديث القائمة بعد السحب
    } else {
        alert('لا يوجد أسماء للاختيار منها!');
    }
});

// إعادة تعيين جميع الأسماء
resetButton.addEventListener('click', () => {
    allNames = [];
    lastTenNames = [];
    otherNames = [];
    updateList();  // إعادة تعيين القائمة
});