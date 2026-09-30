# -html-github

nventory = {
    "P001": {"name": "Laptop Pro 15", "price": 35000.0, "stock": 10},
    "P002": {"name": "Wireless Mouse", "price": 850, "stock": 50},
    "P003": {"name": "Mechanical Keyboard", "price": 2500, "stock": 25},
    "P004": {"name": "4K Monitor 27\"", "price": 12900, "stock": 8},
    "P005": {"name": "USB-C Hub Multi", "price": 1500, "stock": 40}
}

# 2. ฟังก์ชันแสดงรายการสินค้าทั้งหมดในคลัง
def display_inventory():
    print(f"\n{'='*15} รายการสินค้าในคลัง {'='*15}")
    print(f"{'รหัส':<6} | {'ชื่อสินค้า':<22} | {'ราคา (บาท)':<10} | {'คงเหลือ'}")
    print("-" * 55)
    for p_id, info in inventory.items():
        print(f"{p_id:<6} | {info['name']:<22} | {info['price']:>10,.2f} | {info['stock']:>5}")
    print("=" * 55)

# 3. ฟังก์ชันคำนวณและสร้างใบเสร็จรับเงินสำหรับการสั่งซื้อ
def process_order(cart):
    print(f"\n{'*'*15} ใบเสร็จรับเงินจำลอง {'*'*15}")
    subtotal = 0.0
    items_count = 0
    
    # วนลูปรายการสินค้าในตะกร้าเพื่อคำนวณเงินและตัดสต็อก
    for item in cart:
        p_id = item["id"]
        qty = item["qty"]
        
        # ตรวจสอบว่ามีสินค้านี้ในคลังหรือไม่
        if p_id in inventory:
            product = inventory[p_id]
            # ตรวจสอบว่าสินค้ามีพอขายหรือไม่
            if product["stock"] >= qty:
                # ทำการหักสต็อกสินค้า
                product["stock"] -= qty
                item_total = product["price"] * qty
                subtotal += item_total
                items_count += qty
                print(f"- {product['name']:<22} x{qty:<3} : {item_total:>10,.2f} บาท")
            else:
                print(f"❌ สินค้า {product['name']} มีไม่พอ (เหลือเพียง {product['stock']})")
        else:
            print(f"❌ ไม่พบรหัสสินค้า {p_id} ในระบบ")
            
    # หากไม่มีสินค้าที่สั่งซื้อสำเร็จเลยให้ยกเลิกใบเสร็จ
    if items_count == 0:
        print("\n>>> ไม่มีรายการชำระเงินที่สมบูรณ์ <<<")
        print("*" * 50)
        return

    # คำนวณภาษีมูลค่าเพิ่ม (VAT 7%) และยอดสุทธิ
    vat_rate = 0.07
    calculated_vat = subtotal * vat_rate
    grand_total = subtotal + calculated_vat
    
    # พิมพ์สรุปยอดเงินท้ายใบเสร็จ
    print("-" * 50)
    print(f"{'ยอดรวมราคาสินค้า (Subtotal):':<32} {subtotal:>12,.2f} บาท")
    print(f"{'ภาษีมูลค่าเพิ่ม (VAT 7%):':<32} {calculated_vat:>12,.2f} บาท")
    print(f"{'ยอดรวมสุทธิ (Grand Total):':<32} {grand_total:>12,.2f} บาท")
    print(f"{'*'*20} ขอบคุณที่ใช้บริการ {'*'*20}\n")

# 4. ฟังก์ชันเพิ่มสต็อกสินค้าเข้าคลัง (Restock)
def restock_product(p_id, quantity):
    if p_id in inventory:
        inventory[p_id]["stock"] += quantity
        print(f"🟢 เติมสินค้า {inventory[p_id]['name']} สำเร็จ! สต็อกใหม่: {inventory[p_id]['stock']}")
    else:
        print(f"❌ ไม่พบรหัสสินค้า {p_id} สำหรับการเติมสต็อก")

# 5. เริ่มต้นโปรแกรมจำลองการทำงาน
print("🚀 ยินดีต้อนรับเข้าสู่ระบบจัดการร้านค้าจำลอง (Simulation Store)")

# แสดงสถานะคลังสินค้าเริ่มต้น
display_inventory()

# จำลองตะกร้าสินค้าของลูกค้าคนที่ 1 (ซื้อสินค้าปกติที่มีในสต็อก)
customer_cart_1 = [
    {"id": "P001", "qty": 1},
    {"id": "P002", "qty": 2},
    {"id": "P005", "qty": 1}
]
print("\n🛒 [ลูกค้าคนที่ 1] กำลังทำการสั่งซื้อสินค้า...")
process_order(customer_cart_1)

# จำลองตะกร้าสินค้าของลูกค้าคนที่ 2 (ซื้อสินค้าเกินสต็อกที่มีอยู่)
customer_cart_2 = [
    {"id": "P004", "qty": 10}, # ในสต็อกมีแค่ 8
    {"id": "P003", "qty": 3}
]
print("🛒 [ลูกค้าคนที่ 2] กำลังทำการสั่งซื้อสินค้า...")
process_order(customer_cart_2)

# จำลองเจ้าหน้าที่ร้านค้าเติมสต็อกสินค้าที่หมด
print("📦 [เจ้าหน้าที่คลัง] ตรวจพบสต็อกสินค้า P004 ต่ำกว่ากำหนด")
restock_product("P004", 15)

# แสดงสถานะคลังสินค้าล่าสุดหลังจากผ่านกิจกรรมทั้งหมด
print("\n📊 ตรวจสอบยอดสต็อกคงเหลือปัจจุบัน")
display_inventory()
