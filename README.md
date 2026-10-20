<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tiệm Trà Nhỏ - Boba Master</title>
    <style>
        * {
            box-sizing: border-box;
            font-family: 'Nunito', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        body {
            background-color: #f7ebe1;
            margin: 0;
            padding: 12px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }
        .game-card {
            width: 100%;
            max-width: 420px;
            background: #fffdf9;
            border-radius: 24px;
            padding: 16px;
            box-shadow: 0 10px 25px rgba(181, 131, 141, 0.2);
            border: 4px solid #ffcad4;
        }
        /* THANH HEADER TRẠNG THÁI */
        .status-header {
            background: linear-gradient(135deg, #ff9aa2, #ffb7b2);
            padding: 10px 16px;
            border-radius: 16px;
            color: white;
            font-weight: 800;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 14px;
            box-shadow: 0 4px 10px rgba(255, 154, 162, 0.4);
            margin-bottom: 14px;
        }
        .status-item {
            display: flex;
            align-items: center;
            gap: 4px;
        }

        /* KHUNG KHÁCH ORDER */
        .order-bubble {
            background: #ffffff;
            border: 3px solid #ffb7b2;
            padding: 12px 14px;
            border-radius: 18px;
            margin-bottom: 14px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.03);
            position: relative;
        }
        .customer-title {
            color: #b5838d;
            font-weight: 800;
            font-size: 14px;
            margin-bottom: 4px;
        }
        .order-desc {
            color: #5d4037;
            font-size: 13px;
            line-height: 1.4;
            margin: 0;
        }

        /* TIÊU ĐỀ CÁC TRẠM */
        .station-title {
            background: linear-gradient(90deg, #b5838d, #e5989b);
            color: white;
            padding: 6px 12px;
            border-radius: 12px;
            font-size: 12px;
            font-weight: 800;
            margin: 12px 0 8px 0;
            display: inline-block;
            box-shadow: 0 2px 6px rgba(181, 131, 141, 0.2);
        }

        /* KHUNG Ô SELECT ĐÔI & BA */
        .select-row {
            display: flex;
            gap: 8px;
            margin-bottom: 10px;
        }
        select {
            flex: 1;
            padding: 8px 10px;
            border-radius: 12px;
            border: 2px solid #ffcad4;
            background: #fffaf5;
            font-weight: 700;
            color: #6d597a;
            font-size: 12px;
            outline: none;
            cursor: pointer;
            box-shadow: 0 2px 5px rgba(0,0,0,0.02);
        }

        /* BẢNG 12 Ô LƯỚI TOPPING CÓ HÌNH ẢNH MINH HỌA */
        .topping-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 8px;
            margin-bottom: 12px;
            background: #fff5f5;
            padding: 10px;
            border-radius: 16px;
            border: 2px dashed #ffb7b2;
        }
        .topping-card {
            background: #ffffff;
            border: 2px solid #ffcad4;
            border-radius: 14px;
            padding: 6px 2px;
            text-align: center;
            font-size: 10px;
            font-weight: 700;
            color: #5d4037;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 3px 6px rgba(0,0,0,0.03);
            user-select: none;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 4px;
        }
        .topping-card img {
            width: 32px;
            height: 32px;
            object-fit: contain;
        }
        .topping-card:hover {
            transform: translateY(-2px);
        }
        .topping-card.active {
            background: linear-gradient(135deg, #ff9aa2, #e5989b);
            color: white;
            border-color: #b5838d;
            box-shadow: 0 4px 10px rgba(229, 152, 155, 0.4);
            transform: scale(1.04);
        }

        /* NÚT GIAO TRÀ */
        .btn-deliver {
            background: linear-gradient(135deg, #ff9aa2, #b5838d);
            color: white;
            border: none;
            width: 100%;
            padding: 14px;
            border-radius: 16px;
            font-weight: 800;
            font-size: 15px;
            cursor: pointer;
            margin-top: 10px;
            box-shadow: 0 6px 15px rgba(181, 131, 141, 0.4);
            transition: all 0.2s;
        }
        .btn-deliver:active {
            transform: scale(0.98);
        }

        /* THÔNG BÁO */
        #result-status {
            text-align: center;
            font-weight: 800;
            margin-top: 10px;
            font-size: 13px;
            min-height: 20px;
        }
    </style>
</head>
<body>

    <div class="game-card">
        <!-- THANH TRẠNG THÁI -->
        <div class="status-header">
            <div class="status-item">📅 Ngày 28</div>
            <div class="status-item">💰 <span id="money-val">856.8</span>k đ</div>
            <div class="status-item">⭐ 4.7</div>
        </div>

        <!-- BONG BÓNG KHÁCH ĐẶT MÓN -->
        <div class="order-bubble">
            <div class="customer-title" id="cust-name">👧 Khách hàng đang chọn món...</div>
            <p class="order-desc" id="cust-order">Đang tải công thức...</p>
        </div>

        <!-- TRẠM 1: TRÀ & SIZE -->
        <div class="station-title">🧋 1. CHỌN VÒI TRÀ & SIZE</div>
        <div class="select-row">
            <select id="select-tea">
                <option value="Trà sữa">🧋 Trà sữa</option>
                <option value="Matcha">🍵 Matcha</option>
                <option value="Hồng trà">🫖 Hồng trà</option>
                <option value="Lục trà">🍃 Lục trà</option>
                <option value="Oolong">🍵 Oolong</option>
                <option value="Trà thái">🍹 Trà thái</option>
            </select>
            <select id="select-size">
                <option value="Size M">🥤 Size M</option>
                <option value="Size L">🥤 Size L</option>
            </select>
        </div>

        <!-- TRẠM 2: 12 Ô LƯỚI TOPPING CÓ HÌNH MÔ PHỎNG -->
        <div class="station-title">🍮 2. KHAY TOPPING (12 Ô LƯỚI)</div>
        <div class="topping-grid" id="grid-container">
            <!-- 12 Ô Topping kèm Icon/Hình ảnh tự động tạo từ JavaScript -->
        </div>

        <!-- TRẠM 3: FOAM, ĐÁ & ĐƯỜNG -->
        <div class="station-title">🥛 3. FOAM, ĐÁ & ĐƯỜNG</div>
        <div class="select-row">
            <select id="select-foam">
                <option value="Không">Chưa chọn Foam</option>
                <option value="Foam cheese">🧀 Foam cheese</option>
                <option value="Foam matcha">🍵 Foam matcha</option>
                <option value="Foam muối">🧂 Foam muối</option>
            </select>
            <select id="select-sugar">
                <option value="0% đường">0% đường</option>
                <option value="50% đường">50% đường</option>
                <option value="100% đường">100% đường</option>
            </select>
            <select id="select-ice">
                <option value="Không đá">Không đá</option>
                <option value="Ít đá">Ít đá</option>
                <option value="Đá bình thường">Đá bình thường</option>
            </select>
        </div>

        <!-- NÚT HOÀN THÀNH -->
        <button class="btn-deliver" onclick="checkDrinkOrder()">🚀 GIAO LY TRÀ CHO KHÁCH</button>
        <div id="result-status"></div>
    </div>

    <script>
        // Danh sách 12 topping kèm biểu tượng/icon hình ảnh sinh động
        const toppingData = [
            { name: "Trân châu đen", img: "https://cdn-icons-png.flaticon.com/512/3081/3081986.png" },
            { name: "Tàu hủ đen", img: "https://cdn-icons-png.flaticon.com/512/2515/2515183.png" },
            { name: "Jellien", img: "https://cdn-icons-png.flaticon.com/512/3571/3571612.png" },
            { name: "Hếti", img: "https://cdn-icons-png.flaticon.com/512/3081/3081925.png" },
            { name: "Jăn chiến", img: "https://cdn-icons-png.flaticon.com/512/1046/1046784.png" },
            { name: "Thái", img: "https://cdn-icons-png.flaticon.com/512/2921/2921822.png" },
            { name: "Bình tường", img: "https://cdn-icons-png.flaticon.com/512/3132/3132693.png" },
            { name: "Đuôn", img: "https://cdn-icons-png.flaticon.com/512/2480/2480822.png" },
            { name: "Pudding trứng", img: "https://cdn-icons-png.flaticon.com/512/2276/2276931.png" },
            { name: "Thạch matcha", img: "https://cdn-icons-png.flaticon.com/512/3050/3050122.png" },
            { name: "Trân châu vàng", img: "https://cdn-icons-png.flaticon.com/512/1046/1046857.png" },
            { name: "Khoai dẻo", img: "https://cdn-icons-png.flaticon.com/512/1135/1135058.png" }
        ];

        let currentOrder = {};
        let activeTopping = "";
        let currentMoney = 856.8;

        // Render 12 ô topping dạng lưới với Icon hình ảnh
        const grid = document.getElementById("grid-container");
        toppingData.forEach(top => {
            const card = document.createElement("div");
            card.className = "topping-card";
            card.innerHTML = `<img src="${top.img}" alt="${top.name}"><span>${top.name}</span>`;
            card.onclick = () => {
                document.querySelectorAll(".topping-card").forEach(c => c.classList.remove("active"));
                card.classList.add("active");
                activeTopping = top.name;
            };
            grid.appendChild(card);
        });

        // Tạo đơn hàng ngẫu nhiên
        function createRandomOrder() {
            const customers = ["Bé Na 👧", "Minh Quân 👦", "Hà An 👩", "Gia Hân 🌸", "Tuấn Khải ☕"];
            const sizes = ["Size M", "Size L"];
            const teas = ["Trà sữa", "Matcha", "Hồng trà", "Lục trà", "Oolong", "Trà thái"];
            const foams = ["Không", "Foam cheese", "Foam matcha", "Foam muối"];
            const sugars = ["0% đường", "50% đường", "100% đường"];
            const ices = ["Không đá", "Ít đá", "Đá bình thường"];

            const randomTop = toppingData[Math.floor(Math.random() * toppingData.length)].name;

            currentOrder = {
                name: customers[Math.floor(Math.random() * customers.length)],
                size: sizes[Math.floor(Math.random() * sizes.length)],
                tea: teas[Math.floor(Math.random() * teas.length)],
                topping: randomTop,
                foam: foams[Math.floor(Math.random() * foams.length)],
                sugar: sugars[Math.floor(Math.random() * sugars.length)],
                ice: ices[Math.floor(Math.random() * ices.length)],
                reward: Math.floor(Math.random() * 25) + 45
            };

            document.getElementById("cust-name").innerText = `${currentOrder.name} đang gọi món:`;
            document.getElementById("cust-order").innerHTML = 
                `"Cho mình 1 ly <b>${currentOrder.tea}</b> (${currentOrder.size}), topping <b>${currentOrder.topping}</b>, <b>${currentOrder.foam}</b>, <b>${currentOrder.sugar}</b> và <b>${currentOrder.ice}</b> nha!"`;
        }

        // Kiểm tra và giao trà
        function checkDrinkOrder() {
            const userTea = document.getElementById("select-tea").value;
            const userSize = document.getElementById("select-size").value;
            const userFoam = document.getElementById("select-foam").value;
            const userSugar = document.getElementById("select-sugar").value;
            const userIce = document.getElementById("select-ice").value;
            const statusEl = document.getElementById("result-status");

            if (
                userTea === currentOrder.tea &&
                userSize === currentOrder.size &&
                activeTopping === currentOrder.topping &&
                userFoam === currentOrder.foam &&
                userSugar === currentOrder.sugar &&
                userIce === currentOrder.ice
            ) {
                currentMoney += currentOrder.reward / 1000;
                document.getElementById("money-val").innerText = currentMoney.toFixed(1);
                statusEl.style.color = "#2a9d8f";
                statusEl.innerText = `🎉 Pha đúng rồi! Bạn nhận được +${currentOrder.reward}k đ`;
                setTimeout(() => {
                    statusEl.innerText = "";
                    createRandomOrder();
                }, 1400);
            } else {
                statusEl.style.color = "#e76f51";
                statusEl.innerText = "❌ Pha chưa đúng yêu cầu của khách rồi!";
            }
        }

        createRandomOrder();
    </script>
</body>
</html>
