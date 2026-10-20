<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tiệm Trà Nhỏ - Boba Game</title>
    <style>
        * {
            box-sizing: border-box;
            font-family: 'Nunito', 'Segoe UI', Arial, sans-serif;
            margin: 0;
            padding: 0;
        }

        body {
            background-color: #fceade;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 12px;
        }

        .game-card {
            width: 100%;
            max-width: 420px;
            background: #ffffff;
            border-radius: 24px;
            padding: 16px;
            box-shadow: 0 12px 30px rgba(181, 131, 141, 0.25);
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
            margin-bottom: 12px;
        }

        /* KHUNG KHÁCH ORDER */
        .order-bubble {
            background: #fffaf5;
            border: 2px solid #ffb7b2;
            padding: 10px 14px;
            border-radius: 16px;
            margin-bottom: 12px;
        }

        .customer-title {
            color: #b5838d;
            font-weight: 800;
            font-size: 13px;
            margin-bottom: 2px;
        }

        .order-desc {
            color: #5d4037;
            font-size: 12px;
            line-height: 1.4;
        }

        /* KHUNG LY TRÀ MÔ PHỎNG */
        .preview-box {
            background: #fff5f5;
            border: 2px dashed #ffcad4;
            border-radius: 16px;
            padding: 10px;
            display: flex;
            justify-content: center;
            align-items: center;
            margin-bottom: 12px;
            height: 140px;
        }

        .cup-container {
            width: 80px;
            height: 110px;
            border: 3px solid #5d4037;
            border-top: none;
            border-radius: 0 0 16px 16px;
            position: relative;
            background: #ffffff;
            overflow: hidden;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
        }

        .cup-straw {
            position: absolute;
            top: -15px;
            right: 25px;
            width: 10px;
            height: 120px;
            background: #ff9aa2;
            z-index: 10;
            border-radius: 4px;
        }

        .cup-foam {
            height: 20px;
            background: #fff;
            width: 100%;
            border-bottom: 2px solid #e0e0e0;
            display: none;
        }

        .cup-tea {
            width: 100%;
            height: 70%;
            background: #d4a373;
            transition: background 0.3s;
            position: relative;
            display: flex;
            align-items: flex-end;
            justify-content: center;
            padding-bottom: 5px;
        }

        .cup-topping {
            font-size: 18px;
        }

        /* TIÊU ĐỀ TRẠM */
        .station-title {
            background: linear-gradient(90deg, #b5838d, #e5989b);
            color: white;
            padding: 4px 10px;
            border-radius: 10px;
            font-size: 11px;
            font-weight: 800;
            margin: 8px 0 6px 0;
            display: inline-block;
        }

        /* CHỌN OPTION */
        .select-row {
            display: flex;
            gap: 6px;
            margin-bottom: 8px;
        }

        select {
            flex: 1;
            padding: 6px 8px;
            border-radius: 10px;
            border: 2px solid #ffcad4;
            background: #fffaf5;
            font-weight: 700;
            color: #6d597a;
            font-size: 11px;
            outline: none;
        }

        /* KHAY 12 TOPPING */
        .topping-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 6px;
            margin-bottom: 8px;
            background: #fff5f5;
            padding: 8px;
            border-radius: 14px;
            border: 2px dashed #ffb7b2;
        }

        .topping-card {
            background: #ffffff;
            border: 2px solid #ffcad4;
            border-radius: 10px;
            padding: 6px 2px;
            text-align: center;
            font-size: 10px;
            font-weight: 700;
            color: #5d4037;
            cursor: pointer;
            user-select: none;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 2px;
        }

        .topping-card .icon {
            font-size: 18px;
        }

        .topping-card.active {
            background: linear-gradient(135deg, #ff9aa2, #e5989b);
            color: white;
            border-color: #b5838d;
            transform: scale(1.05);
        }

        /* NÚT GIAO TRÀ */
        .btn-deliver {
            background: linear-gradient(135deg, #ff9aa2, #b5838d);
            color: white;
            border: none;
            width: 100%;
            padding: 12px;
            border-radius: 14px;
            font-weight: 800;
            font-size: 14px;
            cursor: pointer;
            margin-top: 6px;
            box-shadow: 0 4px 12px rgba(181, 131, 141, 0.3);
        }

        #result-status {
            text-align: center;
            font-weight: 800;
            margin-top: 8px;
            font-size: 12px;
            min-height: 18px;
        }
    </style>
</head>
<body>

    <div class="game-card">
        <!-- HEADER -->
        <div class="status-header">
            <div>📅 Ngày 28</div>
            <div>💰 <span id="money-val">856.8</span>k đ</div>
            <div>⭐ 4.7</div>
        </div>

        <!-- KHÁCH ORDER -->
        <div class="order-bubble">
            <div class="customer-title" id="cust-name">👧 Khách đang chọn món...</div>
            <div class="order-desc" id="cust-order">Đang tải công thức...</div>
        </div>

        <!-- LY TRÀ MÔ PHỎNG -->
        <div class="preview-box">
            <div class="cup-container">
                <div class="cup-straw"></div>
                <div class="cup-foam" id="cup-foam-layer"></div>
                <div class="cup-tea" id="cup-tea-layer">
                    <span class="cup-topping" id="cup-topping-layer"></span>
                </div>
            </div>
        </div>

        <!-- TRẠM 1: CHỌN TRÀ & SIZE -->
        <div class="station-title">🧋 1. CHỌN TRÀ & SIZE</div>
        <div class="select-row">
            <select id="select-tea" onchange="updatePreview()">
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

        <!-- TRẠM 2: 12 Ô TOPPING -->
        <div class="station-title">🍮 2. KHAY TOPPING (12 Ô LƯỚI)</div>
        <div class="topping-grid" id="grid-container"></div>

        <!-- TRẠM 3: FOAM, ĐÁ & ĐƯỜNG -->
        <div class="station-title">🥛 3. FOAM, ĐÁ & ĐƯỜNG</div>
        <div class="select-row">
            <select id="select-foam" onchange="updatePreview()">
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

        <button class="btn-deliver" onclick="checkDrinkOrder()">🚀 GIAO LY TRÀ CHO KHÁCH</button>
        <div id="result-status"></div>
    </div>

    <script>
        const toppingData = [
            { name: "Trân châu đen", icon: "🧋" },
            { name: "Tàu hủ đen", icon: "🍮" },
            { name: "Jellien", icon: "🍡" },
            { name: "Hếti", icon: "🍯" },
            { name: "Jăn chiến", icon: "🍩" },
            { name: "Thái", icon: "🍧" },
            { name: "Bình tường", icon: "🧊" },
            { name: "Đuôn", icon: "🥥" },
            { name: "Pudding trứng", icon: "🟡" },
            { name: "Thạch matcha", icon: "🍵" },
            { name: "Trân châu vàng", icon: "⭐" },
            { name: "Khoai dẻo", icon: "🍠" }
        ];

        const teaColors = {
            "Trà sữa": "#d4a373",
            "Matcha": "#a3b18a",
            "Hồng trà": "#bc6c25",
            "Lục trà": "#ccd5ae",
            "Oolong": "#ddb892",
            "Trà thái": "#ffb703"
        };

        let currentOrder = {};
        let activeTopping = "";
        let activeToppingIcon = "";
        let currentMoney = 856.8;

        const grid = document.getElementById("grid-container");
        toppingData.forEach(top => {
            const card = document.createElement("div");
            card.className = "topping-card";
            card.innerHTML = `<span class="icon">${top.icon}</span><span>${top.name}</span>`;
            card.onclick = () => {
                document.querySelectorAll(".topping-card").forEach(c => c.classList.remove("active"));
                card.classList.add("active");
                activeTopping = top.name;
                activeToppingIcon = top.icon;
                updatePreview();
            };
            grid.appendChild(card);
        });

        function updatePreview() {
            const teaVal = document.getElementById("select-tea").value;
            const foamVal = document.getElementById("select-foam").value;
            
            document.getElementById("cup-tea-layer").style.background = teaColors[teaVal] || "#d4a373";
            document.getElementById("cup-topping-layer").innerText = activeToppingIcon;

            const foamLayer = document.getElementById("cup-foam-layer");
            if (foamVal !== "Không" && foamVal !== "Chưa chọn Foam") {
                foamLayer.style.display = "block";
                foamLayer.style.background = foamVal === "Foam matcha" ? "#e9edc9" : "#ffffff";
            } else {
                foamLayer.style.display = "none";
            }
        }

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
        updatePreview();
    </script>
</body>
</html>
