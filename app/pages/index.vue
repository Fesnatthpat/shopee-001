<template>
    <div class="min-h-screen bg-slate-100 py-8 px-4 font-sans flex justify-center">
        <div class="w-full max-w-5xl bg-white rounded-2xl shadow-lg overflow-hidden flex flex-col lg:flex-row">

            <!-- ฝั่งซ้าย: ฟอร์มกรอกข้อมูล -->
            <div class="w-full lg:w-3/5 p-6 md:p-8 border-b lg:border-b-0 lg:border-r border-slate-200">
                <h1 class="text-2xl font-bold text-slate-800 mb-6 flex items-center gap-2">
                    📦 ระบบวิเคราะห์ความคุ้มค่ารอบงาน
                </h1>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <!-- กลุ่มข้อมูลรถและระยะทาง -->
                    <div class="space-y-4">
                        <h2 class="text-lg font-semibold text-blue-600 border-b pb-2">เส้นทางและน้ำมัน</h2>
                        <div>
                            <label class="block text-sm text-slate-600 mb-1">ค่ารอบที่ได้รับ (บาท)</label>
                            <input v-model.number="form.payout" type="number"
                                class="w-full p-2.5 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none" />
                        </div>
                        <div>
                            <label class="block text-sm text-slate-600 mb-1">ระยะทางรวม (กม.)</label>
                            <input v-model.number="form.distance" type="number"
                                class="w-full p-2.5 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none" />
                        </div>
                        <div>
                            <label class="block text-sm text-slate-600 mb-1">ราคาน้ำมันปัจจุบัน (บาท/ลิตร)</label>
                            <input v-model.number="form.fuelPrice" type="number"
                                class="w-full p-2.5 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none" />
                        </div>
                        <div>
                            <label class="block text-sm text-slate-600 mb-1">อัตราสิ้นเปลืองรถคุณ (กม./ลิตร)</label>
                            <input v-model.number="form.fuelConsumption" type="number"
                                class="w-full p-2.5 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none"
                                placeholder="เช่น 40" />
                        </div>
                    </div>

                    <!-- กลุ่มข้อมูลพัสดุและจุดรับส่ง -->
                    <div class="space-y-4">
                        <h2 class="text-lg font-semibold text-blue-600 border-b pb-2">รายละเอียดงาน</h2>
                        <div class="flex gap-2">
                            <div class="w-1/3">
                                <label class="block text-xs text-slate-600 mb-1">กว้าง (ซม.)</label>
                                <input v-model.number="form.boxW" type="number"
                                    class="w-full p-2.5 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none text-sm" />
                            </div>
                            <div class="w-1/3">
                                <label class="block text-xs text-slate-600 mb-1">ยาว (ซม.)</label>
                                <input v-model.number="form.boxL" type="number"
                                    class="w-full p-2.5 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none text-sm" />
                            </div>
                            <div class="w-1/3">
                                <label class="block text-xs text-slate-600 mb-1">สูง (ซม.)</label>
                                <input v-model.number="form.boxH" type="number"
                                    class="w-full p-2.5 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none text-sm" />
                            </div>
                        </div>
                        <div>
                            <label class="block text-sm text-slate-600 mb-1">จำนวนพัสดุ (ชิ้น)</label>
                            <input v-model.number="form.itemCount" type="number"
                                class="w-full p-2.5 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none" />
                        </div>
                        <div class="grid grid-cols-2 gap-2">
                            <div>
                                <label class="block text-sm text-slate-600 mb-1">รับกี่ร้าน (จุด)</label>
                                <input v-model.number="form.pickUpCount" type="number"
                                    class="w-full p-2.5 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none" />
                            </div>
                            <div>
                                <label class="block text-sm text-slate-600 mb-1">ส่งกี่ที่ (จุด)</label>
                                <input v-model.number="form.dropOffCount" type="number"
                                    class="w-full p-2.5 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none" />
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- ฝั่งขวา: แสดงผลการวิเคราะห์ -->
            <div class="w-full lg:w-2/5 p-6 md:p-8 bg-slate-50 flex flex-col">
                <h2 class="text-xl font-bold text-slate-800 mb-4">ผลการวิเคราะห์</h2>

                <!-- แถบสีสถานะ -->
                <div :class="[
                    'p-4 rounded-xl text-center mb-6 shadow-sm font-bold text-lg',
                    analysis.isRecommended ? 'bg-green-100 text-green-700 border border-green-200' : 'bg-red-100 text-red-700 border border-red-200'
                ]">
                    {{ analysis.isRecommended ? '✅ งานนี้คุ้มค่า! แนะนำให้รับ' : '❌ ไม่คุ้ม! ควรพิจารณายกเลิก' }}
                </div>

                <div class="space-y-3 flex-grow">
                    <div class="flex justify-between items-center bg-white p-3 rounded-lg border shadow-sm">
                        <span class="text-slate-600">ค่ารอบที่ได้รับ</span>
                        <span class="font-semibold text-blue-600">{{ form.payout }} ฿</span>
                    </div>

                    <div class="flex justify-between items-center bg-white p-3 rounded-lg border shadow-sm">
                        <span class="text-slate-600">หัก ต้นทุนน้ำมัน (ประเมิน)</span>
                        <span class="font-semibold text-red-500">- {{ analysis.fuelCost.toFixed(2) }} ฿</span>
                    </div>

                    <div class="flex justify-between items-center bg-white p-3 rounded-lg border shadow-sm">
                        <span class="text-slate-600">หัก ค่าเสียเวลาหลายจุดแวะ</span>
                        <span class="font-semibold text-red-500">- {{ analysis.stopPenaltyScore }} ฿</span>
                    </div>

                    <!-- แจ้งเตือนขนาดพัสดุ -->
                    <div v-if="analysis.isOverSized"
                        class="bg-orange-50 border border-orange-200 p-3 rounded-lg flex items-start gap-2">
                        <span class="text-orange-500">⚠️</span>
                        <p class="text-xs text-orange-700">ปริมาตรรวมพัสดุใหญ่เกินมาตรฐานมอเตอร์ไซค์
                            อาจรับน้ำหนักหรือจัดเรียงยาก</p>
                    </div>
                </div>

                <!-- สรุปกำไร -->
                <div class="mt-6 pt-6 border-t border-slate-200 text-center">
                    <p class="text-sm text-slate-500 mb-1">กำไรสุทธิโดยประมาณ</p>
                    <p class="text-4xl font-extrabold"
                        :class="analysis.netProfit >= 20 ? 'text-green-600' : 'text-slate-700'">
                        {{ analysis.netProfit.toFixed(2) }} <span class="text-2xl">฿</span>
                    </p>
                    <p class="text-xs text-slate-400 mt-2">**คำนวณจากค่าน้ำมันและจำนวนจุดแวะเท่านั้น</p>
                </div>
            </div>

        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

// 1. สร้าง Interface เพื่อให้ Type ปลอดภัย
interface JobForm {
    payout: number;
    distance: number;
    fuelPrice: number;
    fuelConsumption: number;
    boxW: number;
    boxL: number;
    boxH: number;
    itemCount: number;
    pickUpCount: number;
    dropOffCount: number;
}

// 2. กำหนดค่าเริ่มต้นของฟอร์ม (คุณตั้งค่า Default ไว้ให้ผู้ใช้ได้เลย)
const form = ref<JobForm>({
    payout: 0,
    distance: 0,
    fuelPrice: 0,
    fuelConsumption: 0, // วิ่งได้ 40 กม. ต่อ 1 ลิตร
    boxW: 0,
    boxL: 0,
    boxH: 0,
    itemCount: 0,
    pickUpCount: 0,
    dropOffCount: 0
})

// 3. Logic วิเคราะห์ข้อมูลแบบ Real-time
const analysis = computed(() => {
    const data = form.value;

    // A. คำนวณค่าน้ำมัน: (ระยะทาง / กินน้ำมัน) * ราคาน้ำมัน
    const fuelCost = (data.distance / (data.fuelConsumption || 1)) * data.fuelPrice;

    // B. คำนวณค่าความยุ่งยากจากจุดรับส่ง: ให้จุดรับ/ส่ง ที่เกินมาจาก 1 ที่ มีต้นทุนแฝงจุดละ 10 บาท
    const extraPickUps = Math.max(0, data.pickUpCount - 1);
    const extraDropOffs = Math.max(0, data.dropOffCount - 1);
    const stopPenaltyScore = (extraPickUps * 10) + (extraDropOffs * 10);

    // C. คำนวณขนาดพัสดุรวม: กว้าง x ยาว x สูง x จำนวนชิ้น
    const totalVolumeCm3 = (data.boxW * data.boxL * data.boxH) * data.itemCount;
    // สมมติว่ามอเตอร์ไซค์รับปริมาตรได้ประมาณ 40,000 ลบ.ซม. (ประมาณกล่อง 40x40x25 ซม.)
    const isOverSized = totalVolumeCm3 > 40000;

    // D. คำนวณกำไรสุทธิ
    const netProfit = data.payout - fuelCost - stopPenaltyScore;

    // E. กฎการตัดสินใจ: กำไรสุทธิต้องมากกว่า 20 บาท และของต้องไม่ใหญ่เกินไป
    const isRecommended = netProfit >= 20 && !isOverSized;

    return {
        fuelCost,
        stopPenaltyScore,
        isOverSized,
        netProfit,
        isRecommended
    };
})
</script>