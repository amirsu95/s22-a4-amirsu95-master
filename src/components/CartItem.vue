<template>
  <div class="cart-item" v-if="artwork">

      <div class="cart-preview">
        <framed-artwork :artwork="artwork" :config="cartItem" />
      </div>

      <MuseumLabel :artwork="artwork"></MuseumLabel>
      <slot>
          <div class="cart-frame-description">Small print in a 3.1 cm elegant frame with a 0.6 cm glacier mat.</div>
          <div class="cart-price">€ {{ cartItem.price / 100 }}</div>
          <button class="cart-remove" @click="removeItem"></button>
      </slot>
  </div>
</template>

<script>
import FramedArtwork from "@/components/FramedArtwork";
import * as ArtmartService from "@/services/ArtmartService";
import MuseumLabel from "./MuseumLabel";

export default {
  name: "CartItem",
  components: {
      MuseumLabel,
    FramedArtwork
  },
  props: {
    cartItem: {
      artworkId: Number,
      price: Number,
      printSize: String,
      frameWidth: Number,
      frameStyle: String,
      matWidth: Number,
      matColor: String
    }
  },
  methods: {
    async removeItem(){
        await ArtmartService.deleteCartItem(62);
    }
  },
  data() {
    return {
      artwork: null
    };
  },
  async mounted() {
      this.artwork = await ArtmartService.getArtwork(this.cartItem.artworkId);
  },
  computed: {
    description() {
      let s = { S: "Small", M: "Medium", L: "Large" }[this.cartItem.printSize];
      s += ` print in a ${this.cartItem.frameWidth / 10} cm`;
      s += ` ${this.cartItem.frameStyle} frame`;
      if (this.cartItem.matWidth > 0) {
        s += ` with a ${this.cartItem.matWidth / 10} cm`;
        s += ` ${this.cartItem.matColor} mat`;
      }
      s += ".";
      return s;
    },
    priceText() {
      return (this.cartItem.price / 100).toFixed(2);
    },
    framingRoute() {
      return {
        path: "/framing/" + this.cartItem.artworkId,
        query: {
          printSize: this.cartItem.printSize,
          frameWidth: this.cartItem.frameWidth,
          frameStyle: this.cartItem.frameStyle,
          matWidth: this.cartItem.matWidth,
          matColor: this.cartItem.matColor
        }
      };
    }
  }
};
</script>

<style scoped>
.cart-item {
  margin: 1rem;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}

.cart-preview {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 250px;
  height: 250px;
  justify-content: center;
  flex-shrink: 0;
}

.cart-preview img {
  border: 0px solid black; /* necessary for Chrome & Firefox */
  box-shadow: 0 7px 15px 0 rgba(0, 0, 0, 0.5);
}

.cart-item .museum-label {
  margin-left: 2rem;
  position: relative;
  flex-grow: 1;
  max-width: 500px;
}

.cart-item .museum-label .price {
  font-weight: bold;
  text-align: right;
  margin-top: 1rem;
}

.cart-remove {
  width: 1.5em;
  height: 1.5em;
  padding: 0;
  margin: 0;
  border-radius: 50%;

  position: absolute;
  top: -0.75em;
  right: -0.75em;

  display: flex;
  align-items: center;
  justify-content: center;
}

.cart-remove::before {
  content: "✕";
  font-family: sans-serif;
}

.cart-frame-description {
  margin-top: 1em;
}

.cart-price {
  font-weight: bold;
  text-align: right;
  margin-top: 1em;
}

@media (max-width: 600px) {
  .cart-item {
    flex-direction: column;
  }

  .cart-preview {
    justify-content: flex-end;
  }

  .cart-item .museum-label {
    margin: 0;
    margin-top: 2rem;
  }
}
</style>
