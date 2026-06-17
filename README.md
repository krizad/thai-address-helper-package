# Thai Location Kit

แพ็กเกจสำหรับค้นหาและจัดการข้อมูลที่อยู่ประเทศไทย (ตำบล, อำเภอ, จังหวัด, รหัสไปรษณีย์) ที่มีขนาดเล็ก รวดเร็ว และใช้งานง่าย รองรับทั้ง Node.js และ Browser เขียนด้วย TypeScript

## 🌟 จุดเด่น (Features)

- **ทำงานแบบ In-memory:** โหลดข้อมูลและค้นหาได้อย่างรวดเร็ว ไม่ต้องต่อ API
- **รองรับ TypeScript:** มี Type definition ครบถ้วน (`ThaiAddress`)
- **การค้นหาที่ยืดหยุ่น:** ค้นหาได้ทั้งแบบแยกฟิลด์ หรือค้นหาจากทุกฟิลด์รวมกัน
- **รองรับระบบ Dropdown (Cascading):** ฟังก์ชันสำหรับดึงข้อมูลเป็นลำดับชั้น (จังหวัด -> อำเภอ -> ตำบล -> รหัสไปรษณีย์) สำหรับทำฟอร์มกรอกที่อยู่
- **รองรับการค้นหาด้วยภาษาอังกฤษ:** สามารถค้นหาชื่อตำบล อำเภอ จังหวัด ด้วยภาษาอังกฤษได้

## 📦 การติดตั้ง (Installation)

ติดตั้งผ่าน npm หรือ yarn:

```bash
npm install @krizad/thai-location-kit
# หรือ
yarn add @krizad/thai-location-kit
# หรือ
pnpm add @krizad/thai-location-kit
```

## 🚀 ตัวอย่างการใช้งาน (Usage & Examples)

### 1. การค้นหาข้อมูล (Search)

คุณสามารถค้นหาข้อมูลที่อยู่จากคำค้นหา (Keyword) ได้หลากหลายวิธี

```typescript
import { 
  searchByZipcode, 
  searchBySubDistrict,
  searchAllFields
} from '@krizad/thai-location-kit';

// ค้นหาที่อยู่ทั้งหมดในรหัสไปรษณีย์ 10400
const zipcodeResults = searchByZipcode('10400');
console.log(zipcodeResults);
// Output: [{ subDistrict: 'พญาไท', district: 'พญาไท', province: 'กรุงเทพมหานคร', zipcode: '10400', ... }, ...]

// ค้นหาจากชื่อตำบล (ค้นหาได้ทั้งภาษาไทยและอังกฤษ)
const subDistrictResults = searchBySubDistrict('บางซื่อ');

// ค้นหาจากคำใดๆ (ค้นหาครอบคลุมทุกฟิลด์ เช่น พิมพ์ "ขอนแก่น" หรือ "40000")
const mixedResults = searchAllFields('ขอนแก่น');
```

### 2. การสร้างแบบฟอร์มที่อยู่ (Cascading Dropdowns)

ฟังก์ชันชุดนี้เหมาะสำหรับการทำฟอร์มที่ให้ผู้ใช้เลือก **จังหวัด -> อำเภอ -> ตำบล** ตามลำดับ

```typescript
import { 
  getUniqueProvinces, 
  getDistrictsByProvince, 
  getSubDistrictsByDistrict,
  getZipcodeByHierarchy
} from '@krizad/thai-location-kit';

// สเต็ปที่ 1: ดึงรายชื่อจังหวัดทั้งหมด เพื่อนำไปสร้าง Dropdown จังหวัด
const provinces = getUniqueProvinces(); 
// ['กระบี่', 'กรุงเทพมหานคร', 'กาญจนบุรี', ...]

// สเต็ปที่ 2: เมื่อผู้ใช้เลือกจังหวัด ให้ดึงรายชื่ออำเภอในจังหวัดนั้น
const districtsInBkk = getDistrictsByProvince('กรุงเทพมหานคร');
// ['เขตคลองสาน', 'เขตคลองเตย', 'เขตจตุจักร', ...]

// สเต็ปที่ 3: เมื่อผู้ใช้เลือกอำเภอ ให้ดึงรายชื่อตำบล
const subDistrictsInChatuchak = getSubDistrictsByDistrict('กรุงเทพมหานคร', 'เขตจตุจักร');
/* 
[
  { subDistrict: 'จตุจักร', zipcode: '10900', ... }, 
  { subDistrict: 'จอมพล', zipcode: '10900', ... }
] 
*/

// สเต็ปที่ 4: เติมรหัสไปรษณีย์อัตโนมัติเมื่อเลือกครบ
const zipcode = getZipcodeByHierarchy('กรุงเทพมหานคร', 'เขตจตุจักร', 'จอมพล');
// '10900'
```

### 3. การสร้าง Auto-complete Form จากรหัสไปรษณีย์

หากต้องการให้ผู้ใช้กรอก รหัสไปรษณีย์ หรือ ตำบล แล้วฟอร์มเติมอำเภอและจังหวัดให้อัตโนมัติ:

```typescript
import { cascadeFromZipcode, cascadeFromSubDistrict } from '@krizad/thai-location-kit';

// เมื่อกรอก 10400 จะได้รายการที่อยู่เพื่อนำไปเติมอัตโนมัติ
const addresses = cascadeFromZipcode('10400');

// หรือค้นหาจากตำบล
const addressesFromSub = cascadeFromSubDistrict('ดินแดง');
```

### 4. การเตรียมข้อมูลสำหรับ UI Dropdown

ฟังก์ชัน `formatToDropdownOptions` ช่วยแปลงรูปแบบข้อมูลให้พร้อมใช้งานกับ Component Dropdown ทั่วไป เช่น React-Select

```typescript
import { searchByZipcode, formatToDropdownOptions } from '@krizad/thai-location-kit';

const rawData = searchByZipcode('10400');
const options = formatToDropdownOptions(rawData);

console.log(options);
/* 
[
  {
    label: "ต.พญาไท อ.พญาไท จ.กรุงเทพมหานคร 10400",
    value: "พญาไท|พญาไท|กรุงเทพมหานคร|10400",
    raw: { ...ข้อมูลต้นฉบับ }
  },
  ...
]
*/
```

## 📚 API Reference

### โครงสร้างข้อมูล (Models & Interfaces)

```typescript
export interface ThaiAddress {
  subDistrict: string;    // ชื่อตำบล (ภาษาไทย)
  district: string;       // ชื่ออำเภอ (ภาษาไทย)
  province: string;       // ชื่อจังหวัด (ภาษาไทย)
  zipcode: string;        // รหัสไปรษณีย์
  subDistrictEng: string; // ชื่อตำบล (ภาษาอังกฤษ)
  districtEng: string;    // ชื่ออำเภอ (ภาษาอังกฤษ)
  provinceEng: string;    // ชื่อจังหวัด (ภาษาอังกฤษ)
  subDistrictId: number;  // รหัสตำบล
  districtId: number;     // รหัสอำเภอ
  provinceId: number;     // รหัสจังหวัด
}

export interface DropdownOption {
  label: string;          // ข้อความสำหรับแสดงใน Dropdown
  value: string;          // ค่า Value สำหรับอ้างอิง
  raw: ThaiAddress;       // ข้อมูลที่อยู่ต้นฉบับ
}
```

### หมวดหมู่การค้นหา (Search Functions)

- `searchBySubDistrict(keyword: string): ThaiAddress[]`
- `searchByDistrict(keyword: string): ThaiAddress[]`
- `searchByProvince(keyword: string): ThaiAddress[]`
- `searchByZipcode(keyword: string): ThaiAddress[]`
- `searchAllFields(keyword: string): ThaiAddress[]`

### หมวดหมู่แบบฟอร์มลำดับชั้น (Cascading / Hierarchy)

- `getUniqueProvinces(): string[]`
- `getDistrictsByProvince(province: string): string[]`
- `getSubDistrictsByDistrict(province: string, district: string): ThaiAddress[]`
- `getZipcodeByHierarchy(province: string, district: string, subDistrict: string): string | null`
- `getUniqueZipcodes(): string[]`
- `cascadeFromZipcode(zipcode: string): ThaiAddress[]`
- `cascadeFromSubDistrict(subDistrict: string): ThaiAddress[]`

### หมวดหมู่จัดรูปแบบ UI (UI Formatting)

- `formatToDropdownOptions(addresses: ThaiAddress[]): DropdownOption[]`

## 📄 License

MIT License
