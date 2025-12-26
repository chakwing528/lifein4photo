// ... (保留原本變數) ...
    let currentFormat = '2x6'; // 預設尺寸

    // 在 HTML 中加入這兩個按鈕供選擇
    // <button class="btn" onclick="setFormat('2x6')">設定為 2x6 (直條)</button>
    // <button class="btn" onclick="setFormat('4x6')">設定為 4x6 (大張)</button>

    function setFormat(format) {
        currentFormat = format;
        alert("已切換為: " + format + " 英吋模式");
    }

    // 修改 mergePhotos 函數
    function mergePhotos() {
        let stripWidth, totalHeight, padding, headerHeight, footerHeight, photoHeight;

        if (currentFormat === '2x6') {
            // 2x6 邏輯 (直條)
            stripWidth = 600; 
            padding = 20;
            headerHeight = 50; 
            footerHeight = 100;
            photoHeight = 450; // 4:3 比例
            
            totalHeight = headerHeight + (photoHeight * 4) + (padding * 5) + footerHeight;
            finalCanvas.width = stripWidth;
            finalCanvas.height = totalHeight;

            // 繪製背景
            ctx.fillStyle = "#ffecd2";
            ctx.fillRect(0, 0, stripWidth, totalHeight);

            // 繪製 4 張照片
            photos.forEach((photoCanvas, index) => {
                const y = headerHeight + padding + (index * (photoHeight + padding));
                ctx.drawImage(photoCanvas, padding, y, stripWidth - (padding * 2), photoHeight);
            });
        } else {
            // 4x6 邏輯 (兩條並排)
            // 4x6 英吋 @ 300DPI 約為 1200x1800
            stripWidth = 1200;
            const singleStripWidth = 600;
            padding = 20;
            headerHeight = 50;
            footerHeight = 100;
            photoHeight = 450;
            
            // 高度保持跟 2x6 一樣 (約 1800+ px)
            totalHeight = headerHeight + (photoHeight * 4) + (padding * 5) + footerHeight; 
            
            finalCanvas.width = stripWidth;
            finalCanvas.height = totalHeight;

            ctx.fillStyle = "#ffecd2";
            ctx.fillRect(0, 0, stripWidth, totalHeight);

            // 畫左邊一排
            photos.forEach((photoCanvas, index) => {
                const y = headerHeight + padding + (index * (photoHeight + padding));
                ctx.drawImage(photoCanvas, padding, y, singleStripWidth - (padding * 2), photoHeight);
            });

            // 畫右邊一排 (複製一份)
            photos.forEach((photoCanvas, index) => {
                const y = headerHeight + padding + (index * (photoHeight + padding));
                // X 座標位移 600px
                ctx.drawImage(photoCanvas, singleStripWidth + padding, y, singleStripWidth - (padding * 2), photoHeight);
            });
            
            // 畫中間裁切線 (虛線)
            ctx.beginPath();
            ctx.setLineDash([10, 10]);
            ctx.moveTo(600, 0);
            ctx.lineTo(600, totalHeight);
            ctx.strokeStyle = '#ccc';
            ctx.stroke();
        }

        // ... (保留原本的文字繪製與顯示邏輯) ...
        document.getElementById('result-container').style.display = 'block';
        location.href = "#result-container";
    }
