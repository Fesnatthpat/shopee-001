<template>
    <div class="min-h-screen bg-slate-50 py-6 md:py-10 px-4 font-sans flex justify-center items-start">
        <div class="w-full max-w-6xl bg-white rounded-3xl shadow-xl overflow-hidden flex flex-col lg:flex-row border border-slate-200">

            <!-- ฝั่งซ้าย: ฟอร์มกรอกข้อมูล -->
            <div class="w-full lg:w-3/5 p-6 md:p-10 border-b lg:border-b-0 lg:border-r border-slate-200">
                <div class="flex flex-col md:flex-row md:items-center justify-between mb-8 gap-4">
                    <h1 class="text-2xl md:text-3xl font-extrabold text-slate-800 flex items-center gap-3">
                        <span class="bg-blue-600 text-white p-2 rounded-xl shadow-lg shadow-blue-200">📦</span>
                        วิเคราะห์ความคุ้มค่ารอบงาน
                    </h1>
                    <button @click="resetForm" 
                        class="text-sm font-medium text-slate-400 hover:text-red-500 flex items-center gap-1 transition-colors px-3 py-2 rounded-lg hover:bg-red-50">
                        <span>🔄</span> ล้างข้อมูลทั้งหมด
                    </button>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                    <!-- กลุ่มที่ 1: ข้อมูลการเงินและเชื้อเพลิง -->
                    <div class="space-y-6">
                        <div class="bg-blue-50/50 p-5 rounded-2xl border border-blue-100/50 space-y-4 shadow-sm">
                            <div class="flex items-center gap-2 text-blue-700 pb-2 border-b border-blue-100">
                                <span class="text-xl">💰</span>
                                <h2 class="text-lg font-bold">ข้อมูลการเงินและรถ</h2>
                            </div>
                            
                            <div class="space-y-4">
                                <div>
                                    <label class="block text-sm font-bold text-slate-600 mb-1.5">ค่ารอบที่ได้รับ (บาท)</label>
                                    <input v-model.number="form.payout" type="number"
                                        class="w-full p-3 bg-white border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none transition-all shadow-sm"
                                        placeholder="0.00" />
                                </div>

                                <div class="grid grid-cols-2 gap-3">
                                    <div>
                                        <label class="block text-xs font-bold text-slate-500 mb-1.5">ราคาน้ำมัน (฿/ลิตร)</label>
                                        <input v-model.number="form.fuelPrice" type="number"
                                            class="w-full p-3 bg-white border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none text-sm transition-all" />
                                    </div>
                                    <div>
                                        <label class="block text-xs font-bold text-slate-500 mb-1.5">กินน้ำมัน (กม./ลิตร)</label>
                                        <input v-model.number="form.fuelConsumption" type="number"
                                            class="w-full p-3 bg-white border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none text-sm transition-all"
                                            placeholder="เช่น 40" />
                                    </div>
                                </div>

                                <div>
                                    <label class="block text-sm font-bold text-slate-600 mb-1.5 text-blue-600">ค่าน้ำมันที่เติมวันนี้ (บาท)</label>
                                    <input v-model.number="form.dailyFuelCost" type="number"
                                        class="w-full p-3 bg-blue-50 border border-blue-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none transition-all font-semibold"
                                        placeholder="0.00" />
                                </div>
                            </div>
                        </div>

                        <!-- รายละเอียดพัสดุ -->
                        <div class="bg-slate-50 p-5 rounded-2xl border border-slate-200 space-y-4 shadow-sm">
                            <div class="flex items-center gap-2 text-slate-700 pb-2 border-b border-slate-200">
                                <span class="text-xl">📏</span>
                                <h2 class="text-lg font-bold">ขนาดพัสดุรวม</h2>
                            </div>
                            <div class="flex gap-3">
                                <div class="flex-1">
                                    <label class="block text-[10px] font-bold text-slate-400 uppercase mb-1">กว้าง</label>
                                    <input v-model.number="form.boxW" type="number"
                                        class="w-full p-2.5 bg-white border border-slate-200 rounded-lg focus:ring-2 focus:ring-slate-400 outline-none text-sm transition-all text-center" />
                                </div>
                                <div class="flex-1">
                                    <label class="block text-[10px] font-bold text-slate-400 uppercase mb-1">ยาว</label>
                                    <input v-model.number="form.boxL" type="number"
                                        class="w-full p-2.5 bg-white border border-slate-200 rounded-lg focus:ring-2 focus:ring-slate-400 outline-none text-sm transition-all text-center" />
                                </div>
                                <div class="flex-1">
                                    <label class="block text-[10px] font-bold text-slate-400 uppercase mb-1">สูง</label>
                                    <input v-model.number="form.boxH" type="number"
                                        class="w-full p-2.5 bg-white border border-slate-200 rounded-lg focus:ring-2 focus:ring-slate-400 outline-none text-sm transition-all text-center" />
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- กลุ่มที่ 2: รายละเอียดเส้นทางและน้ำหนัก -->
                    <div class="space-y-6">
                        <!-- ส่วนจุดรับพัสดุ -->
                        <div class="bg-orange-50/50 p-5 rounded-2xl border border-orange-100 space-y-4 shadow-sm">
                            <div class="flex items-center justify-between border-b border-orange-100 pb-2">
                                <div class="flex items-center gap-2 text-orange-700">
                                    <span class="text-xl">🏪</span>
                                    <h2 class="text-lg font-bold">จุดรับพัสดุ</h2>
                                </div>
                                <button @click="addPickUp" class="text-xs font-bold bg-orange-600 text-white px-3 py-1.5 rounded-lg shadow-sm shadow-orange-200 hover:bg-orange-700 active:scale-95 transition-all">
                                    + เพิ่มจุด
                                </button>
                            </div>

                            <div class="flex gap-2 items-center bg-white/60 p-2 rounded-xl border border-orange-100">
                                <label class="text-xs font-bold text-orange-800 px-2 whitespace-nowrap">จำนวนร้าน:</label>
                                <input :value="form.pickUpDistances.length" 
                                    @input="e => updatePickUpCount(parseInt((e.target as HTMLInputElement).value) || 1)"
                                    type="number" min="1"
                                    class="w-full p-1.5 bg-white border border-orange-200 rounded-lg focus:ring-2 focus:ring-orange-500 outline-none text-center text-sm font-bold" />
                            </div>

                            <div class="max-h-40 overflow-y-auto space-y-2 pr-2 custom-scrollbar">
                                <div v-for="(dist, index) in form.pickUpDistances" :key="index" class="flex gap-2 group">
                                    <div class="flex-grow relative">
                                        <span class="absolute left-3 top-1/2 -translate-y-1/2 text-[10px] font-bold text-slate-400">#{{ index + 1 }}</span>
                                        <input v-model.number="form.pickUpDistances[index]" type="number"
                                            class="w-full p-2.5 pl-9 bg-white border border-slate-200 rounded-xl focus:ring-2 focus:ring-orange-400 outline-none text-sm transition-all"
                                            placeholder="ระยะทาง (กม.)" />
                                    </div>
                                    <button v-if="form.pickUpDistances.length > 1" @click="removePickUp(index)" 
                                        class="text-slate-300 hover:text-red-500 p-2 transition-colors">✕</button>
                                </div>
                            </div>
                        </div>

                        <!-- ส่วนจุดส่งพัสดุ -->
                        <div class="bg-green-50/50 p-5 rounded-2xl border border-green-100 space-y-4 shadow-sm">
                            <div class="flex items-center justify-between border-b border-green-100 pb-2">
                                <div class="flex items-center gap-2 text-green-700">
                                    <span class="text-xl">📍</span>
                                    <h2 class="text-lg font-bold">จุดส่งพัสดุ</h2>
                                </div>
                                <button @click="addDestination" class="text-xs font-bold bg-green-600 text-white px-3 py-1.5 rounded-lg shadow-sm shadow-green-200 hover:bg-green-700 active:scale-95 transition-all">
                                    + เพิ่มจุด
                                </button>
                            </div>

                            <div class="flex gap-2 items-center bg-white/60 p-2 rounded-xl border border-green-100">
                                <label class="text-xs font-bold text-green-800 px-2 whitespace-nowrap">จำนวนที่ส่ง:</label>
                                <input :value="form.destinations.length" 
                                    @input="e => updateDropOffCount(parseInt((e.target as HTMLInputElement).value) || 1)"
                                    type="number" min="1"
                                    class="w-full p-1.5 bg-white border border-green-200 rounded-lg focus:ring-2 focus:ring-green-500 outline-none text-center text-sm font-bold" />
                            </div>

                            <div class="max-h-40 overflow-y-auto space-y-2 pr-2 custom-scrollbar">
                                <div v-for="(dist, index) in form.destinations" :key="index" class="flex gap-2 group">
                                    <div class="flex-grow relative">
                                        <span class="absolute left-3 top-1/2 -translate-y-1/2 text-[10px] font-bold text-slate-400">#{{ index + 1 }}</span>
                                        <input v-model.number="form.destinations[index]" type="number"
                                            class="w-full p-2.5 pl-9 bg-white border border-slate-200 rounded-xl focus:ring-2 focus:ring-green-400 outline-none text-sm transition-all"
                                            placeholder="ระยะทาง (กม.)" />
                                    </div>
                                    <button v-if="form.destinations.length > 1" @click="removeDestination(index)" 
                                        class="text-slate-300 hover:text-red-500 p-2 transition-colors">✕</button>
                                </div>
                            </div>
                        </div>

                        <!-- ส่วนน้ำหนักพัสดุ -->
                        <div class="bg-purple-50/50 p-5 rounded-2xl border border-purple-100 space-y-4 shadow-sm">
                            <div class="flex items-center justify-between border-b border-purple-100 pb-2">
                                <div class="flex items-center gap-2 text-purple-700">
                                    <span class="text-xl">⚖️</span>
                                    <h2 class="text-lg font-bold">น้ำหนักพัสดุ</h2>
                                </div>
                                <button @click="addParcel" class="text-xs font-bold bg-purple-600 text-white px-3 py-1.5 rounded-lg shadow-sm shadow-purple-200 hover:bg-purple-700 active:scale-95 transition-all">
                                    + เพิ่มพัสดุ
                                </button>
                            </div>
                            <div class="max-h-40 overflow-y-auto space-y-2 pr-2 custom-scrollbar">
                                <div v-for="(w, index) in form.parcelWeights" :key="index" class="flex gap-2 group">
                                    <div class="flex-grow relative">
                                        <span class="absolute left-3 top-1/2 -translate-y-1/2 text-[10px] font-bold text-slate-400">#{{ index + 1 }}</span>
                                        <input v-model.number="form.parcelWeights[index]" type="number"
                                            class="w-full p-2.5 pl-9 bg-white border border-slate-200 rounded-xl focus:ring-2 focus:ring-purple-400 outline-none text-sm transition-all"
                                            placeholder="น้ำหนัก (กก.)" />
                                    </div>
                                    <button v-if="form.parcelWeights.length > 1" @click="removeParcel(index)" 
                                        class="text-slate-300 hover:text-red-500 p-2 transition-colors">✕</button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- ฝั่งขวา: แสดงผลการวิเคราะห์ -->
            <div class="w-full lg:w-2/5 p-6 md:p-10 bg-slate-50 flex flex-col border-t lg:border-t-0 border-slate-200">
                <div class="flex items-center gap-3 mb-8">
                    <h2 class="text-2xl font-black text-slate-800 tracking-tight uppercase">สรุปผลวิเคราะห์</h2>
                    <div class="h-1 flex-grow bg-slate-200 rounded-full"></div>
                </div>

                <!-- แถบสีสถานะ -->
                <div :class="[
                    'p-6 rounded-2xl text-center mb-8 shadow-md transform transition-all duration-500',
                    analysis.isRecommended ? 'bg-gradient-to-br from-green-500 to-emerald-600 text-white shadow-green-200' : 'bg-gradient-to-br from-rose-500 to-red-600 text-white shadow-red-200'
                ]">
                    <div class="text-3xl mb-2">{{ analysis.isRecommended ? '🏆' : '⚠️' }}</div>
                    <div class="font-black text-xl tracking-wide uppercase">
                        {{ analysis.isRecommended ? 'แนะนำให้รับงานนี้' : 'งานนี้ยังไม่คุ้มค่า' }}
                    </div>
                </div>

                <div class="space-y-4 flex-grow">
                    <!-- Dashboard Cards -->
                    <div class="grid grid-cols-2 gap-3">
                        <div class="bg-white p-4 rounded-2xl shadow-sm border border-slate-100 flex flex-col items-center text-center">
                            <span class="text-[10px] font-bold text-slate-400 uppercase mb-1">ระยะทางรวม</span>
                            <span class="text-xl font-black text-slate-800">{{ analysis.totalDistance.toFixed(1) }} <small class="text-xs">กม.</small></span>
                        </div>
                        <div class="bg-white p-4 rounded-2xl shadow-sm border border-slate-100 flex flex-col items-center text-center">
                            <span class="text-[10px] font-bold text-slate-400 uppercase mb-1">จำนวนพัสดุ</span>
                            <span class="text-xl font-black text-slate-800">{{ analysis.itemCount }} <small class="text-xs">ชิ้น</small></span>
                        </div>
                    </div>

                    <div class="bg-white p-5 rounded-2xl shadow-sm border border-slate-100 space-y-4">
                        <div class="flex justify-between items-center pb-3 border-b border-slate-50">
                            <span class="text-sm font-bold text-slate-500">ค่ารอบที่ได้รับ</span>
                            <span class="text-lg font-bold text-blue-600 tracking-tight">{{ form.payout.toLocaleString() }} ฿</span>
                        </div>

                        <div class="flex justify-between items-center">
                            <span class="text-sm font-bold text-slate-500">ต้นทุนน้ำมัน (ประเมิน)</span>
                            <span class="text-lg font-bold text-rose-500 tracking-tight">- {{ analysis.fuelCost.toFixed(2) }} ฿</span>
                        </div>

                        <div v-if="form.dailyFuelCost > 0" class="pt-3 border-t border-slate-50">
                            <div class="flex justify-between items-center">
                                <span class="text-sm font-bold text-blue-600">น้ำมันที่เติมวันนี้</span>
                                <div class="text-right">
                                    <div class="font-bold text-blue-700 tracking-tight">{{ form.dailyFuelCost.toLocaleString() }} ฿</div>
                                    <div class="text-[10px] font-bold text-slate-400 uppercase">{{ analysis.dailyFuelLiters.toFixed(2) }} ลิตร</div>
                                </div>
                            </div>
                        </div>

                        <div class="pt-3 border-t border-slate-50 flex justify-between items-center">
                            <span class="text-sm font-bold text-slate-500">น้ำหนักพัสดุรวม</span>
                            <span :class="['text-lg font-bold tracking-tight', analysis.isOverWeight ? 'text-rose-600' : 'text-slate-800']">
                                {{ analysis.totalWeight.toFixed(2) }} <small class="text-xs">กก.</small>
                            </span>
                        </div>
                    </div>

                    <!-- แจ้งเตือนความเสี่ยง -->
                    <div class="space-y-3">
                        <div v-if="analysis.isOverSized"
                            class="bg-orange-500 p-4 rounded-xl text-white flex items-start gap-3 shadow-lg shadow-orange-100 animate-pulse">
                            <span class="text-xl">📦</span>
                            <p class="text-xs font-bold leading-relaxed">ของชิ้นใหญ่เกินมาตรฐาน! ระวังจัดเรียงยากหรือบังกระจกมองข้าง</p>
                        </div>

                        <div v-if="analysis.isOverWeight"
                            class="bg-rose-600 p-4 rounded-xl text-white flex items-start gap-3 shadow-lg shadow-rose-100">
                            <span class="text-xl">⚖️</span>
                            <p class="text-xs font-bold leading-relaxed">น้ำหนักเกิน 20 กก.! อันตรายต่อช่วงล่างและระยะเบรค ควรระมัดระวังเป็นพิเศษ</p>
                        </div>
                    </div>
                </div>

                <!-- Footer Summary (Profit) -->
                <div class="mt-8 space-y-4">
                    <div class="bg-white p-6 rounded-3xl border-2 border-slate-100 shadow-inner text-center">
                        <p class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-1">กำไรสุทธิรอบนี้</p>
                        <p class="text-5xl font-black transition-colors duration-500"
                            :class="analysis.netProfit >= 20 ? 'text-green-600' : 'text-slate-400'">
                            {{ analysis.netProfit.toFixed(2) }} <span class="text-xl">฿</span>
                        </p>
                    </div>

                    <div v-if="form.dailyFuelCost > 0" 
                        class="bg-gradient-to-br from-blue-600 to-indigo-700 p-6 rounded-3xl text-white shadow-xl shadow-blue-100 text-center relative overflow-hidden group">
                        <div class="absolute top-0 right-0 p-4 opacity-10 group-hover:scale-125 transition-transform">💰</div>
                        <p class="text-[10px] font-black opacity-80 uppercase tracking-widest mb-1">เงินคงเหลือจริงในกระเป๋า</p>
                        <p class="text-4xl font-black">
                            {{ analysis.netProfitAfterFuelPaid.toFixed(2) }} <span class="text-xl">฿</span>
                        </p>
                        <p class="text-[10px] font-bold opacity-60 mt-2 italic">ยอดหลังหักทุนเติมน้ำมันทั้งวัน</p>
                    </div>
                </div>
            </div>

        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, watch } from 'vue'

// 1. สร้าง Interface เพื่อให้ Type ปลอดภัย
interface JobForm {
    payout: number;
    destinations: number[];
    fuelPrice: number;
    fuelConsumption: number;
    dailyFuelCost: number;
    boxW: number;
    boxL: number;
    boxH: number;
    parcelWeights: number[];
    pickUpDistances: number[];
}

const DEFAULT_FORM: JobForm = {
    payout: 0,
    destinations: [0],
    fuelPrice: 0,
    fuelConsumption: 40, 
    dailyFuelCost: 0,
    boxW: 0,
    boxL: 0,
    boxH: 0,
    parcelWeights: [0],
    pickUpDistances: [0]
}

// 2. กำหนดค่าเริ่มต้นของฟอร์ม
const form = ref<JobForm>({ ...DEFAULT_FORM })

// ระบบ Persistence
onMounted(() => {
    const saved = localStorage.getItem('shopeex_job_data')
    if (saved) {
        try {
            const parsed = JSON.parse(saved)
            form.value = { ...DEFAULT_FORM, ...parsed }
        } catch (e) {
            console.error('Failed to load saved data', e)
        }
    }
})

watch(form, (newVal) => {
    localStorage.setItem('shopeex_job_data', JSON.stringify(newVal))
}, { deep: true })

const resetForm = () => {
    if (confirm('คุณต้องการล้างข้อมูลทั้งหมดใช่หรือไม่?')) {
        form.value = { 
            ...DEFAULT_FORM,
            destinations: [0],
            parcelWeights: [0],
            pickUpDistances: [0]
        }
    }
}

const addDestination = () => form.value.destinations.push(0)
const removeDestination = (index: number) => {
    if (form.value.destinations.length > 1) form.value.destinations.splice(index, 1)
}

const updateDropOffCount = (count: number) => {
    const current = form.value.destinations.length
    if (count > current) {
        for (let i = 0; i < count - current; i++) form.value.destinations.push(0)
    } else if (count < current && count >= 1) {
        form.value.destinations.splice(count)
    }
}

const addParcel = () => form.value.parcelWeights.push(0)
const removeParcel = (index: number) => {
    if (form.value.parcelWeights.length > 1) form.value.parcelWeights.splice(index, 1)
}

const addPickUp = () => form.value.pickUpDistances.push(0)
const removePickUp = (index: number) => {
    if (form.value.pickUpDistances.length > 1) form.value.pickUpDistances.splice(index, 1)
}

const updatePickUpCount = (count: number) => {
    const current = form.value.pickUpDistances.length
    if (count > current) {
        for (let i = 0; i < count - current; i++) form.value.pickUpDistances.push(0)
    } else if (count < current && count >= 1) {
        form.value.pickUpDistances.splice(count)
    }
}

// 3. Logic วิเคราะห์ข้อมูลแบบ Real-time
const analysis = computed(() => {
    const data = form.value;

    const pickUpTotalDist = data.pickUpDistances.reduce((sum, dist) => sum + (dist || 0), 0);
    const dropOffTotalDist = data.destinations.reduce((sum, dist) => sum + (dist || 0), 0);
    const totalDistance = pickUpTotalDist + dropOffTotalDist;

    const fuelCost = (totalDistance / (data.fuelConsumption || 1)) * data.fuelPrice;
    const itemCount = data.parcelWeights.length;

    const totalVolumeCm3 = (data.boxW * data.boxL * data.boxH) * itemCount;
    const isOverSized = totalVolumeCm3 > 40000;

    const totalWeight = data.parcelWeights.reduce((sum, w) => sum + (w || 0), 0);
    const isOverWeight = totalWeight > 20;

    const netProfit = data.payout - fuelCost;
    const dailyFuelLiters = data.fuelPrice > 0 ? data.dailyFuelCost / data.fuelPrice : 0;
    const netProfitAfterFuelPaid = netProfit - data.dailyFuelCost;

    const isRecommended = netProfit >= 20 && !isOverSized && !isOverWeight;

    return {
        totalDistance,
        fuelCost,
        itemCount,
        dailyFuelLiters,
        netProfitAfterFuelPaid,
        isOverSized,
        isOverWeight,
        totalWeight,
        netProfit,
        isRecommended
    };
})
</script>

<style scoped>
.custom-scrollbar::-webkit-scrollbar {
    width: 4px;
}
.custom-scrollbar::-webkit-scrollbar-track {
    background: transparent;
}
.custom-scrollbar::-webkit-scrollbar-thumb {
    background: #e2e8f0;
    border-radius: 10px;
}
.custom-scrollbar::-webkit-scrollbar-thumb:hover {
    background: #cbd5e1;
}
</style>