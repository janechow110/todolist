<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>眼镜款式管理</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: "Microsoft YaHei", sans-serif;
    }
    body {
      background: #f7f8fa;
      padding: 20px;
      max-width: 1000px;
      margin: 0 auto;
    }
    .title {
      font-size: 22px;
      margin-bottom: 20px;
      color: #333;
    }
    .tool-bar {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
      flex-wrap: wrap;
    }
    #search {
      flex: 1;
      padding: 10px 14px;
      border: 1px solid #ddd;
      border-radius: 8px;
      outline: none;
    }
    button {
      padding: 10px 16px;
      border-radius: 8px;
      border: none;
      background: #4e6ef2;
      color: white;
      cursor: pointer;
    }
    .gallery {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
      gap: 12px;
    }
    .item {
      background: white;
      border-radius: 10px;
      overflow: hidden;
      box-shadow: 0 2px 6px #00000008;
    }
    .item img {
      width: 100%;
      height: 160px;
      object-fit: cover;
      display: block;
    }
    .info {
      padding: 10px;
      font-size: 14px;
    }
    .name {
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }
    .empty {
      grid-column: 1/-1;
      text-align: center;
      padding: 60px 0;
      color: #999;
    }
  </style>
</head>

<body>
  <div class="title">眼镜款式管理</div>

  <div class="tool-bar">
    <input type="text" id="search" placeholder="搜索眼镜名称">
    <button id="upload">上传图片</button>
    <button id="searchByImg">以图搜图</button>
  </div>

  <input type="file" id="fileInput" accept="image/*" multiple hidden>
  <div class="gallery" id="list"></div>

  <script>
    const upload = document.getElementById('upload');
    const fileInput = document.getElementById('fileInput');
    const list = document.getElementById('list');
    const search = document.getElementById('search');
    const searchByImg = document.getElementById('searchByImg');

    // 读取
    function get() {
      return JSON.parse(localStorage.getItem('glasses') || '[]');
    }

    // 保存
    function save(arr) {
      localStorage.setItem('glasses', JSON.stringify(arr));
    }

    // 渲染
    function render(key = '') {
      const data = get();
      const f = data.filter(i => i.name?.includes(key));
      list.innerHTML = '';

      if (f.length === 0) {
        list.innerHTML = '<div class="empty">暂无眼镜</div>';
        return;
      }

      f.forEach(item => {
        const div = document.createElement('div');
        div.className = 'item';
        div.innerHTML = `
          <img src="${item.src}" alt="眼镜">
          <div class="info">
            <div class="name">${item.name}</div>
          </div>
        `;
        list.appendChild(div);
      });
    }

    // 转BASE64
    function toBase64(file) {
      return new Promise((res) => {
        const a = new FileReader();
        a.onload = e => res(e.target.result);
        a.readAsDataURL(file);
      });
    }

    // 上传
    upload.onclick = () => fileInput.click();
    fileInput.onchange = async (e) => {
      const files = Array.from(e.target.files);
      const data = get();
      for (const f of files) {
        const src = await toBase64(f);
        data.unshift({ name: f.name, src });
      }
      save(data);
      render(search.value);
    };

    // 搜索
    search.oninput = () => render(search.value.trim());

    // 以图搜图
    searchByImg.onclick = () => {
      const i = document.createElement('input');
      i.type = 'file';
      i.accept = 'image/*';
      i.onchange = () => alert('已选择图片，可对接搜图接口');
      i.click();
    };

    render();
  </script>
</body>
</html>
