<template>
  <div class="wallet-container">
    <div class="card-box flex-start">
      <div class="card" v-for="item in countryWallet" :key="item.country">
        <div class="top">{{ $t('wallet.Wallet Balance') }}</div>
        <div class="middle flex-between">
          <div>
            <img :src="item.flag" alt="Country flag" class="countryflag" />
            <div class="currencyName">{{ item.currency }}</div>
          </div>
          <!-- 货币余额 -->
          <div>
            <span>{{ item.walletBalance }}</span>
            <span class="currencyUnit">{{ item.currency }}</span>
          </div>
        </div>
        <div class="bottom flex-between">
          <el-button type="primary" size="small" class="btn-blue">{{ $t('wallet.Add Money') }}</el-button>
          <el-button type="primary" size="small" class="btn-blue">{{ $t('wallet.Wallet Transactions') }}</el-button>
          <el-button type="primary" size="small" class="btn-blue">{{ $t('wallet.Withdraw') }}</el-button>
        </div>
      </div>
    </div>
    <div class="record">
      <div class="title">{{ $t('wallet.Latest Wallet Transactions') }}</div>
      <div class="table">
        <el-table :data="tableData" style="width: 100%">
          <el-table-column prop="type" :label="$t('wallet.Type')" width="180" />
          <el-table-column prop="date" :label="$t('wallet.Date')" width="180">
            <template #default="scope"> {{ formatTime(scope.row.date) }}</template>
          </el-table-column>
          <el-table-column prop="amount" :label="$t('wallet.Amount')" width="180" />
        </el-table>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { formatTime } from '@/utils/format'

const countryWallet = [
  {
    country: 'USA',
    currency: 'USD',
    flag: 'https://images.unsplash.com/photo-1499200493734-6ba25a83f77c?q=80&w=2940&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    walletBalance: 12345.67
  },
  {
    country: 'USA1',
    currency: 'USD',
    flag: 'https://images.unsplash.com/photo-1499200493734-6ba25a83f77c?q=80&w=2940&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    walletBalance: 12345.67
  },
  {
    country: 'USA2',
    currency: 'USD',
    flag: 'https://images.unsplash.com/photo-1499200493734-6ba25a83f77c?q=80&w=2940&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    walletBalance: 12345.67
  },
  {
    country: 'USA4',
    currency: 'USD',
    flag: 'https://images.unsplash.com/photo-1499200493734-6ba25a83f77c?q=80&w=2940&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    walletBalance: 12345.67
  }
]
// let tableData = ref<any[]>([])
let tableData = [
  {
    type: 'Load Balance',
    date: 1719995520000,
    amount: '1000'
  },
  {
    type: 'Load Balance',
    date: 1719995520000,
    amount: '1000'
  },
  {
    type: 'Load Balance',
    date: 1719995520000,
    amount: '1000'
  }
]
</script>

<style lang="scss" scoped>
.wallet-container {
  .countryflag {
    width: 40px;
    height: 40px;
    border-radius: 50%;
  }
  .card-box {
    flex-direction: row;
    flex-wrap: wrap;
    gap: 20px;
    background-color: #f2f2f2;
    padding: 20px;
    border-radius: 10px;
    @media (max-width: 768px) {
      .card {
        width: calc(100% - 40px);
      }
    }
    .card {
      border-radius: 10px;
      background-color: white;
      padding: 20px;
      box-shadow: 0px 4px 8px 0px rgba(0, 0, 0, 0.2);
      transition: transform 0.3s;
      &:hover {
        transform: scale(1.05);
      }
      &:nth-child(1) {
        // margin-right: 10px;
      }
      &:nth-child(3) {
        // margin-left: 10px;
      }

      .bottom {
        gap: 5px;
        flex-wrap: wrap;
        .wallet-button {
          height: 30px !important;
          width: fit-content !important;
          cursor: pointer !important;
        }

        ::v-deep .el-button {
          // 应用于 <button> 元素
          height: 30px !important;
          width: fit-content !important;
          cursor: pointer !important;
        }
        ::v-deep .el-button * {
          cursor: pointer !important;
        }
        .el-button + .el-button {
          margin-left: 0px !important;
        }
      }
    }
  }
}
</style>
