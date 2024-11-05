<script lang="ts">
    import { Button } from "$lib/components/ui/button";
    import { Card } from "$lib/components/ui/card";
    import { Input } from "$lib/components/ui/input";
    import { Label } from "$lib/components/ui/label";
    import { RadioGroup, RadioGroupItem } from "$lib/components/ui/radio-group";
    import { Textarea } from "$lib/components/ui/textarea";
    import { Checkbox } from "$lib/components/ui/checkbox";
    import { Check } from "lucide-svelte";
    import { fade } from "svelte/transition";
  
    let formData = {
      firstName: "",
      lastName: "",
      email: "",
      queryType: "",
      message: " ",
      consent: false
    };
  
    let errors: Record<string, string> = {};
    let showSuccess = false;
  
    function validateForm() {
      errors = {};
      
      if (!formData.firstName) errors.firstName = "This field is required";
      if (!formData.lastName) errors.lastName = "This field is required";
      if (!formData.email) {
        errors.email = "This field is required";
      } else if (!/\S+@\S+\.\S+/.test(formData.email)) {
        errors.email = "Please enter a valid email address";
      }
      if (!formData.queryType) errors.queryType = "Please select a query type";
      if (!formData.message || formData.message.trim() === "") errors.message = "This field is required";
      if (!formData.consent) errors.consent = "To submit this form, please consent to being contacted";
  
      return Object.keys(errors).length === 0;
    }
  
    async function handleSubmit() {
      if (validateForm()) {
        // Simulate API call
        await new Promise(resolve => setTimeout(resolve, 1000));
        showSuccess = true;
        setTimeout(() => showSuccess = false, 5000);
        
        // Reset form
        formData = {
          firstName: "",
          lastName: "",
          email: "",
          queryType: "",
          message: " ",
          consent: false
        };
      }
    }
  </script>
  
  <div class="min-h-screen bg-[#e6f4ea] flex items-center justify-center p-4">
    {#if showSuccess}
      <div 
        class="fixed top-4 left-1/2 -translate-x-1/2 bg-[#1e3a34] text-white px-6 py-4 rounded-lg shadow-lg flex items-center gap-2 z-50"
        transition:fade
      >
        <Check class="w-5 h-5" />
        <div>
          <h4 class="font-semibold">Message Sent!</h4>
          <p class="text-sm opacity-90">Thanks for completing the form. We'll be in touch soon!</p>
        </div>
      </div>
    {/if}
  
    <Card class="w-full max-w-lg p-6 bg-white shadow-lg">
      <form on:submit|preventDefault={handleSubmit} class="space-y-6">
        <h2 class="text-2xl font-semibold text-gray-900">Contact Us</h2>
  
        <div class="flex flex-col md:flex-row gap-4">
          <div class="flex-1 space-y-2">
            <Label for="firstName" class="flex gap-1">
              First Name <span class="text-red-500">*</span>
            </Label>
            <Input
              type="text"
              id="firstName"
              class="h-11 {errors.firstName ? 'border-red-500' : ''}"
            />
            {#if errors.firstName}
              <p class="text-sm text-red-500">{errors.firstName}</p>
            {/if}
          </div>
  
          <div class="flex-1 space-y-2">
            <Label for="lastName" class="flex gap-1">
              Last Name <span class="text-red-500">*</span>
            </Label>
            <Input
              type="text"
              id="lastName"
              class="h-11 {errors.lastName ? "border-red-500" : ""}"
            />
            {#if errors.lastName}
              <p class="text-sm text-red-500">{errors.lastName}</p>
            {/if}
          </div>
        </div>
  
        <div class="space-y-2">
          <Label for="email" class="flex gap-1">
            Email Address <span class="text-red-500">*</span>
          </Label>
          <Input
            type="email"
            id="email"
            class="h-11 {errors.email ? 'border-red-500' : ''}"
          />
          {#if errors.email}
            <p class="text-sm text-red-500">{errors.email}</p>
          {/if}
        </div>
  
        <div class="space-y-2">
          <Label class="flex gap-1">
            Query Type <span class="text-red-500">*</span>
          </Label>
          <RadioGroup value={formData.queryType} class="w-full flex flex-col md:flex-row gap-4">
            <div class="flex items-center space-x-2 w-full lg:w-1/2 border p-4 rounded-lg">
              <RadioGroupItem value="general" id="general" />
              <Label for="general">General Enquiry</Label>
            </div>
            <div class="flex items-center space-x-2 w-full lg:w-1/2 border p-4 rounded-lg">
              <RadioGroupItem value="support" id="support" />
              <Label for="support">Support Request</Label>
            </div>
          </RadioGroup>
          {#if errors.queryType}
            <p class="text-sm text-red-500">{errors.queryType}</p>
          {/if}
        </div>
  
        <div class="space-y-2">
          <Label for="message" class="flex gap-1">
            Message <span class="text-red-500">*</span>
          </Label>
          <Textarea
            id="message"
            class={`min-h-[120px] ${errors.message ? "border-red-500" : ""}`}
          />
          {#if errors.message}
            <p class="text-sm text-red-500">{errors.message}</p>
          {/if}
        </div>
  
        <div class="space-y-2">
          <div class="flex items-start gap-2">
            <Checkbox
              id="consent"
              checked={formData.consent}
              onCheckedChange={(checked) => formData.consent = checked}
              class={errors.consent ? "border-red-500" : ""}
            />
            <Label for="consent" class="text-sm leading-tight">
              I consent to being contacted by the team <span class="text-red-500">*</span>
            </Label>
          </div>
          {#if errors.consent}
            <p class="text-sm text-red-500">{errors.consent}</p>
          {/if}
        </div>
  
        <Button type="submit" class="w-full h-12 bg-emerald-700 hover:bg-emerald-800 text-center">
          Submit
        </Button>
      </form>
    </Card>
  </div>
