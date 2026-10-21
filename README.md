function fetchDataFromGoogleSheet() {
    document.getElementById('recordCount').innerText = "กำลังดึงข้อมูลจาก Google Sheets...";

    fetch(GOOGLE_SCRIPT_URL)
        .then(response => response.json())
        .then(data => {
            console.log("ข้อมูลดิบที่ได้รับจาก Apps Script:", data); // เปิด F12 ดูกลุ่มข้อมูลตรงนี้ได้ครับ

            rawData = data.map((item, index) => {
                // ดึงชื่อคอลัมน์ทั้งหมดที่เป็นไปได้จาก Object เพื่อความชัวร์
                const keys = Object.keys(item);
                
                // ใช้เทคนิคหาค่าจาก Key ที่ใกล้เคียงที่สุด หรือใช้ตำแหน่งคอลัมน์
                let rawDate = item["วันที่พบปัญหา"] || item[keys[0]] || "-";
                let dept = item["แผนกที่พบปัญหา"] || item["แผนก"] || item[keys[1]] || "ไม่ระบุ";
                let mc = item["เครื่อง"] || item["หมายเลขเครื่องจักร"] || item[keys[2]] || "ไม่ระบุ";
                let shift = item["กะการทำงาน"] || item["กะ"] || item[keys[3]] || "-";
                let operator = item["หัวหน้าเครื่อง"] || item["ผู้ควบคุม"] || item[keys[4]] || "-";
                let job = item["เลขที่ใบสั่งผลิต"] || item["JOB"] || item[keys[5]] || "-";
                let problem = item["ปัญหาที่พบ"] || item["ปัญหา"] || item[keys[6]] || "-";
                let qtyPcs = Number(item["จำนวนของเสีย (ชิ้น)"] || item["จำนวนชิ้น"] || item[keys[7]]) || 0;
                let qtySheets = Number(item["จำนวนขอ(แผ่น)"] || item["จำนวนของเสีย (แผ่น)"] || item["จำนวนแผ่น"] || item[keys[8]]) || 0;
                
                let rawMonth = item["เดือน"] ? item["เดือน"].toString().trim() : "";
                if (rawMonth.startsWith("มกราคม") && !rawMonth.includes(" ")) rawMonth = "มกราคม 69";
                if (rawMonth.startsWith("พฤษภาคม") && !rawMonth.includes(" ")) rawMonth = "พฤษภาคม 69";

                return {
                    id: index + 1,
                    date: rawDate.toString().slice(0, 10),
                    dept_norm: dept.toString().trim(),
                    machine: mc.toString().trim(),
                    shift: shift.toString().trim(),
                    operator: operator.toString().trim(),
                    job: job.toString().trim(),
                    problem: problem.toString().trim(),
                    qty_pcs: qtyPcs,
                    qty_sheets: qtySheets,
                    month: rawMonth || "ตุลาคม 69"
                };
            });

            populateDropdowns();
            renderCharts();
            applyFilters();
        })
        .catch(error => {
            console.error("Error fetching data:", error);
            alert("ไม่สามารถดึงข้อมูลได้ กรุณาตรวจสอบสิทธิ์การแชร์ของ Apps Script หรือรีเฟรชอีกครั้งค่ะ");
        });
}
