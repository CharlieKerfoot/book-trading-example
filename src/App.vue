<script lang="ts">
import NavBar from './components/NavBar.vue'
import type { Trade, OutgoingTrade, Book } from '../shared/book'
export default {
  components: { NavBar },
  data() {
    return {
      incomingTrades: [] as Trade[],
      outgoingTrades: [] as OutgoingTrade[],
      tradeHistory: [] as Trade[],
    }
  },
  created() {
    // TODO: make a request to the server to get the active trades
    this.incomingTrades = [
      {
        id: 1,
        book: {
          cover: 'https://m.media-amazon.com/images/I/8125BDk3l9L._SL1500_.jpg',
          id: 3,
          title: 'The Catcher in the Rye',
          author: 'J.D. Salinger'
        },
        offeredBy: 'Jane Smith',
        status: 'pending',
        createdAt: '2025-01-31'
      }
    ];
    this.outgoingTrades = [
      {
        id: 2,
        book: {
          id: 4,
          title: '1984',
          author: 'George Orwell',
          cover: 'https://m.media-amazon.com/images/I/61NAx5pd6XL.jpg'
        },
        offeredBy: 'John Doe',
        offeredTo: 'Jane Smith',
        status: 'pending',
        createdAt: '2025-01-30'
      }
    ]
    this.tradeHistory = [
      {
        id: 1,
        book: {
          id: 1,
          title: 'The Great Gatsby',
          author: 'F. Scott Fitzgerald',
          cover: 'https://i0.wp.com/americanwritersmuseum.org/wp-content/uploads/2018/02/CK-3.jpg?resize=267%2C400&ssl=1'
        },
        createdAt: '2025-01-10',
        status: 'accepted',
        offeredBy: 'Someone'
      }
    ]
  },
  methods: {
    createTrade(trade: OutgoingTrade) {
      console.log("test")
      this.outgoingTrades.push(trade)
    },
    cancelTrade(trade: OutgoingTrade) {
      this.outgoingTrades = this.outgoingTrades.filter((currTrade) => !(currTrade == trade));
    },
    acceptTrade(trade: Trade) {
      this.incomingTrades = this.incomingTrades.filter((currTrade) => !(currTrade == trade));
      this.tradeHistory.push({...trade, status: "accepted"})
    },
    declineTrade(trade: Trade) {
      this.incomingTrades = this.incomingTrades.filter((currTrade) => !(trade == currTrade));
      this.tradeHistory.push({...trade, status: "declined"})
    },
    proposeTrade(book: Book): void {
      const trade: OutgoingTrade = {
                book: book,
                offeredTo: "",
                status: 'pending',
                createdAt: new Date().toISOString(),
                id: 0,
                offeredBy: '' // TODO: get the current user
      }       
      this.outgoingTrades.push(trade)
    }
  }
}
</script>


<template>

  <NavBar></NavBar>


  <RouterView @create-trade="createTrade" @cancel-trade="cancelTrade" @accept-trade="acceptTrade" @decline-trade="declineTrade" @propose-trade="proposeTrade"
   :tradeHistory="tradeHistory" :outgoingTrades="outgoingTrades" :incomingTrades="incomingTrades" />
</template>
