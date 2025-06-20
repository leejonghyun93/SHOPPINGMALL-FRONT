<template>
  <div class="main-row">
    <!-- 대표 이미지 -->
    <div class="product-image-area">
      <img :src="product.image" alt="대표이미지" />
    </div>

    <!-- 상품 정보 테이블 -->
    <div class="product-meta-table">
      <div class="table-wrapper">
        <table>
          <tbody>
            <tr><th>상품번호</th><td>{{ product.id }}</td></tr>
            <tr><th>상품명</th><td>{{ product.name }}</td></tr>
            <tr><th>카테고리</th><td>{{ product.category }}</td></tr>
            <tr><th>상품가격</th><td>{{ product.price.toLocaleString() }}원</td></tr>
            <tr><th>판매가격</th><td>{{ product.salePrice.toLocaleString() }}원</td></tr>
            <tr><th>재고수량</th><td>{{ product.stock }}</td></tr>
            <tr><th>상품 조회수</th><td>{{ product.views }}</td></tr>
            <tr><th>판매량</th><td>{{ product.sold }}</td></tr>
            <tr><th>후기수</th><td>{{ product.reviews }}</td></tr>
            <tr><th>판매 상태</th><td>{{ product.status }}</td></tr>
            <tr><th>등록날짜</th><td>{{ product.createdAt }}</td></tr>
            <tr><th>수정날짜</th><td>{{ product.updatedAt }}</td></tr>
          </tbody>
        </table>

        <div class="bottom-actions">
          <button class="btn edit-btn" @click="goToEdit">수정</button>
          <button class="btn list-btn" @click="goToList">목록</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue';
import { useRouter, useRoute } from 'vue-router';
import ProductDetailLayout from './ProductDetailLayout.vue';

// props로 productCode 받기 (구조분해 할당으로 변수 선언)
const { productCode } = defineProps({
  productCode: String
});

const router = useRouter();
const route = useRoute();

const tabs = ['상품 상세정보', '후기', '문의'];

// 현재 탭을 URL 경로에 따라 결정
const currentTab = ref('상품 상세정보');

function updateCurrentTabByRoute() {
  const path = route.path;
  if (path.endsWith('/reviews')) currentTab.value = '후기';
  else if (path.endsWith('/inquiries')) currentTab.value = '문의';
  else currentTab.value = '상품 상세정보';
}

updateCurrentTabByRoute();

watch(() => route.path, () => {
  updateCurrentTabByRoute();
});

function onTabClick(tab) {
  currentTab.value = tab;

  const basePath = `/product/${productCode}`;
  if (tab === '상품 상세정보') {
    router.push(basePath);
  } else if (tab === '후기') {
    router.push(`${basePath}/reviews`);
  } else if (tab === '문의') {
    router.push(`${basePath}/inquiries`);
  }
}

function goToList() {
  router.push({ name: 'ProductList' });
}

function goToEdit() {
  router.push({ name: 'ProductEdit', params: { id: productCode } });
}

// 임시 더미 데이터 (실제 API 호출 필요)
const product = {
  id: productCode,
  image: 'https://via.placeholder.com/540x540?text=대표이미지',
  name: '예시 상품명',
  category: '카테고리명',
  price: 25000,
  salePrice: 22000,
  stock: 37,
  views: 1234,
  sold: 57,
  reviews: 2,
  status: '판매중',
  createdAt: '2025-06-01',
  updatedAt: '2025-06-14'
};
</script>

<style scoped>
/* 기존 스타일 유지 */
.main-row {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  gap: 140px;
  margin-top: 40px;
  margin-bottom: 20px;
  width: 100%;
}
.product-image-area {
  flex: 0 0 540px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.product-image-area img {
  width: 650px;
  height: 650px;
  object-fit: cover;
  border-radius: 10px;
  border: 1px solid #e0e0e0;
  background: #fafbfc;
}
.product-meta-table {
  flex: 1 1 0;
  min-width: 0;
  max-width: 700px;
  margin-left: 40px;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
}
.table-wrapper {
  display: flex;
  flex-direction: column;
  height: 100%;
  position: relative;
}
.product-meta-table table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 0;
  font-size: 1.08rem;
  border: 1.5px solid #c7d1e0;
}
.product-meta-table th,
.product-meta-table td {
  padding: 14px 18px;
  border: 1px solid #c7d1e0;
  background: #fff;
}
.product-meta-table th {
  background: #f7faff;
  width: 120px;
  font-weight: 600;
}
.bottom-actions {
  display: flex;
  flex-direction: row;
  gap: 16px;
  align-items: flex-end;
  justify-content: flex-end;
  margin-top: 16px;
}
.btn {
  padding: 8px 22px;
  border: none;
  border-radius: 6px;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.18s;
}
.list-btn {
  background: #eceff7;
  color: #333;
}
.list-btn:hover {
  background: #dde2ee;
}
.edit-btn {
  background: #3e7eff;
  color: #fff;
}
.edit-btn:hover {
  background: #295fc5;
}
@media (max-width: 1200px) {
  .main-row {
    flex-direction: column;
    gap: 32px;
  }
  .product-image-area img {
    width: 90vw;
    max-width: 520px;
    height: auto;
  }
  .product-meta-table {
    width: 100%;
    max-width: none;
    margin-left: 0;
  }
}
</style>
