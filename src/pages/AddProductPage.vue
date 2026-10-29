<template>
  <div class="container my-5" style="max-width: 500px">
    <h1 class="text-center">
      {{ editMode ? "Edit Product" : "Add New Product" }}
    </h1>

    <form @submit.prevent="submitProduct">
      <!-- Product Name -->
      <div class="mb-3">
        <label for="productNameInput" class="form-label">
          Product Name
        </label>

        <input
          type="text"
          class="form-control"
          id="productNameInput"
          v-model="form.name"
          required
        />
      </div>

      <!-- Description -->
      <div class="mb-3">
        <label for="productDescription" class="form-label">
          Description
        </label>

        <textarea
          class="form-control"
          id="productDescription"
          v-model="form.description"
          rows="5"
          required
        ></textarea>
      </div>

      <!-- Price -->
      <div class="mb-3">
        <label for="productPrice" class="form-label">
          Price
        </label>

        <input
          type="number"
          class="form-control"
          id="productPrice"
          v-model.number="form.price"
          min="0"
          step="0.01"
          required
        />
      </div>

      <!-- Category -->
      <div class="mb-3">
        <label for="productCategory" class="form-label">
          Category
        </label>

        <select
          class="form-select"
          id="productCategory"
          v-model="form.category"
          required
        >
          <option disabled value="">Select category</option>
          <option value="In-Person">In-Person</option>
          <option value="Virtual">Virtual</option>
          <option value="Activity">Activity</option>
          <option value="Meal">Meal</option>
          <option value="Other">Other</option>
        </select>
      </div>

      <!-- Submit -->
      <button
        type="submit"
        class="btn btn-primary w-100"
        :disabled="!isEnabled || loading"
      >
        {{
          loading
            ? "Saving..."
            : editMode
              ? "Update Product"
              : "Create Product"
        }}
      </button>
    </form>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from "vue";
import { useRoute, useRouter } from "vue-router";
import api from "../api";

const route = useRoute();
const router = useRouter();

// Determine whether the form is creating or editing a product
const editMode = computed(() => !!route.params.id);

// Form fields
const form = ref({
  name: "",
  description: "",
  price: null,
  category: "",
});

// Loading state
const loading = ref(false);

// Enable submission only when all required fields are valid
const isEnabled = computed(() => {
  return (
    form.value.name.trim() !== "" &&
    form.value.description.trim() !== "" &&
    form.value.price !== null &&
    form.value.price !== "" &&
    Number.isFinite(Number(form.value.price)) &&
    Number(form.value.price) >= 0 &&
    form.value.category !== ""
  );
});

// Fetch existing product when editing
const fetchProduct = async () => {
  try {
    loading.value = true;

    const { data } = await api.get(`/products/${route.params.id}`);

    form.value = {
      name: data.name ?? "",
      description: data.description ?? "",
      price: data.price ?? null,
      category: data.category ?? "",
    };
  } catch (err) {
    console.error("Product not found:", err);
    router.push("/products/all");
  } finally {
    loading.value = false;
  }
};

// Create or update product
const submitProduct = async () => {
  if (!isEnabled.value || loading.value) return;

  try {
    loading.value = true;

    if (editMode.value) {
      await api.patch(
        `/products/${route.params.id}/update`,
        form.value
      );
    } else {
      await api.post("/products", form.value);
    }

    router.push("/products/all");
  } catch (err) {
    console.error("Product submission failed:", err);

    // Log the server's response when available
    console.error("Server response:", err.response?.data);
  } finally {
    loading.value = false;
  }
};

// Load the product only when editing
onMounted(() => {
  if (editMode.value) {
    fetchProduct();
  }
});
</script>
