# Shopping History & Monthly Reports Feature
## Smart Grocery Manager - Feature Planning Document

**Created:** 2025-11-29  
**Status:** 📋 Planning  
**Priority:** High  

---

## 🎯 Feature Overview

Enable users to save completed shopping trips with actual purchases (including impulse buys), track spending over time, and generate monthly reports.

### Key Capabilities
1. **Complete Trip Workflow** - Reconcile planned vs actual purchases
2. **Extra Items** - Add items bought that weren't on the list
3. **Price Corrections** - Update catalog with actual prices
4. **30-Day History** - Rolling history with automatic cleanup
5. **Monthly Reports** - Spending insights and analytics

---

## ✅ User Decisions

| Question | Decision |
|----------|----------|
| Should extra items be added to catalog? | ✅ **Yes** |
| Want a notes field for each trip? | ✅ **Yes** |
| Price corrections update catalog? | ✅ **Yes** |
| History retention period | **30 days** |

---

## 🔄 User Flow

```
┌─────────────────────────────────────────────────────────────┐
│                    SHOPPING TRIP LIFECYCLE                   │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. CREATE LIST                                              │
│     └── Add items to shopping list from catalog              │
│                                                              │
│  2. GO SHOPPING                                              │
│     └── Buy items (+ impulse purchases)                      │
│                                                              │
│  3. AFTER TRIP - RECONCILE                                   │
│     ├── ☑️ Check items actually purchased                    │
│     ├── ➕ Add extra items bought (not on list)              │
│     ├── 💰 Correct prices if different                       │
│     └── 📝 Add trip notes                                    │
│                                                              │
│  4. COMPLETE & SAVE                                          │
│     ├── Save to 30-day history                               │
│     ├── Add extra items to catalog (optional)                │
│     ├── Update catalog prices (if corrected)                 │
│     └── Clear shopping list                                  │
│                                                              │
│  5. VIEW HISTORY & REPORTS                                   │
│     ├── See past 30 days of trips                            │
│     └── Monthly spending summary                             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 📊 Data Structure

### Shopping History Schema

```json
{
  "shopping_history": [
    {
      "id": "trip-2025-11-29-tj-001",
      "store": "Trader Joe's",
      "date": "2025-11-29T14:30:00",
      "status": "completed",
      
      "planned_items": [
        {
          "id": "tj-1",
          "name": "Organic Bananas",
          "category": "Produce",
          "planned_qty": 2,
          "actual_qty": 2,
          "catalog_price": 0.99,
          "actual_price": 0.99,
          "purchased": true
        },
        {
          "id": "tj-8",
          "name": "Greek Yogurt",
          "category": "Dairy",
          "planned_qty": 2,
          "actual_qty": 0,
          "catalog_price": 1.99,
          "actual_price": null,
          "purchased": false,
          "skip_reason": "Out of stock"
        }
      ],
      
      "extra_items": [
        {
          "id": "extra-001",
          "name": "Chocolate Bar",
          "category": "Snacks",
          "qty": 2,
          "price": 2.49,
          "added_to_catalog": true,
          "catalog_id": "tj-50"
        },
        {
          "id": "extra-002",
          "name": "Impulse Chips",
          "category": "Snacks",
          "qty": 1,
          "price": 3.99,
          "added_to_catalog": false
        }
      ],
      
      "totals": {
        "planned_total": 5.96,
        "actual_planned_total": 1.98,
        "extra_total": 8.97,
        "trip_total": 10.95,
        "savings": 0.00,
        "items_planned": 2,
        "items_purchased": 1,
        "items_skipped": 1,
        "extra_items_count": 2
      },
      
      "notes": "Store was out of yogurt. Found great deal on chocolate!",
      
      "price_corrections": [
        {
          "item_id": "tj-22",
          "item_name": "Dark Chocolate",
          "old_price": 2.49,
          "new_price": 2.99,
          "catalog_updated": true
        }
      ]
    }
  ],
  
  "history_metadata": {
    "retention_days": 30,
    "last_cleanup": "2025-11-29T00:00:00",
    "total_trips_all_time": 45
  }
}
```

---

## 🎨 UI Design

### Complete Trip Modal

```
┌─────────────────────────────────────────────────────────────┐
│ ✅ Complete Shopping Trip - Trader Joe's                     │
│ 📅 November 29, 2025                                    [X]  │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│ 📋 PLANNED ITEMS                                             │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ [✓] Organic Bananas     Qty: [2]  Price: [$0.99]        │ │
│ │ [✓] Avocados            Qty: [3]  Price: [$1.49]        │ │
│ │ [ ] Greek Yogurt        Qty: [2]  Price: [$1.99]        │ │
│ │     └── Skip reason: [Out of stock     ▼]               │ │
│ └─────────────────────────────────────────────────────────┘ │
│                                                              │
│ ➕ ADD EXTRA ITEMS (bought but not on list)                  │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ Search catalog: [chocolate________] or Add custom       │ │
│ │                                                          │ │
│ │ Name:     [Chocolate Bar        ]                        │ │
│ │ Category: [Snacks          ▼]                            │ │
│ │ Price:    [$2.49  ]  Qty: [2]                            │ │
│ │ [✓] Add to catalog for future use                        │ │
│ │                                                          │ │
│ │ [➕ Add Item]                                             │ │
│ └─────────────────────────────────────────────────────────┘ │
│                                                              │
│ 📝 EXTRA ITEMS ADDED                                         │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ • Chocolate Bar x2 = $4.98  [🗑️]                         │ │
│ │ • Chips x1 = $3.99  [🗑️]                                 │ │
│ └─────────────────────────────────────────────────────────┘ │
│                                                              │
│ 📝 TRIP NOTES                                                │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ Store was out of yogurt. Will get next time.            │ │
│ │ Found a great deal on chocolate!                        │ │
│ └─────────────────────────────────────────────────────────┘ │
│                                                              │
│ ═══════════════════════════════════════════════════════════ │
│                                                              │
│ 💰 TRIP SUMMARY                                              │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ Planned Items:        $6.45  (2 of 3 purchased)         │ │
│ │ Extra Items:          $8.97  (2 items)                  │ │
│ │ ─────────────────────────────────────                   │ │
│ │ TRIP TOTAL:          $15.42                             │ │
│ │                                                          │ │
│ │ 📊 Impulse spending: 58% of total                       │ │
│ └─────────────────────────────────────────────────────────┘ │
│                                                              │
│         [💾 Complete & Save Trip]    [❌ Cancel]             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### History Tab

```
┌─────────────────────────────────────────────────────────────┐
│ 📅 Shopping History                                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│ Filter: [All Stores ▼]  Month: [November 2025 ▼]            │
│                                                              │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ 📅 Nov 29 | 🏪 Trader Joe's | 💰 $15.42 | 📦 4 items     │ │
│ │    └── Planned: $6.45 | Extra: $8.97                    │ │
│ │    └── Note: "Store was out of yogurt..."          [▼]  │ │
│ ├─────────────────────────────────────────────────────────┤ │
│ │ 📅 Nov 25 | 🏪 Safeway      | 💰 $32.50 | 📦 8 items     │ │
│ │    └── Planned: $28.00 | Extra: $4.50                   │ │
│ │    └── Note: "Weekly groceries"                    [▼]  │ │
│ ├─────────────────────────────────────────────────────────┤ │
│ │ 📅 Nov 20 | 🏪 Costco       | 💰 $125.00| 📦 12 items    │ │
│ │    └── Planned: $110.00 | Extra: $15.00                 │ │
│ │    └── Note: "Monthly bulk shopping"               [▼]  │ │
│ └─────────────────────────────────────────────────────────┘ │
│                                                              │
│ ═══════════════════════════════════════════════════════════ │
│                                                              │
│ 📊 NOVEMBER 2025 SUMMARY                                     │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ Total Spent:     $172.92                                │ │
│ │ Total Trips:     3                                      │ │
│ │ Average/Trip:    $57.64                                 │ │
│ │                                                          │ │
│ │ By Store:                                               │ │
│ │   Trader Joe's:  $15.42  (1 trip)                       │ │
│ │   Safeway:       $32.50  (1 trip)                       │ │
│ │   Costco:        $125.00 (1 trip)                       │ │
│ │                                                          │ │
│ │ Planned vs Actual:                                      │ │
│ │   Planned:       $144.45 (84%)                          │ │
│ │   Extra/Impulse: $28.47  (16%)                          │ │
│ └─────────────────────────────────────────────────────────┘ │
│                                                              │
│ [📥 Download Report]  [📧 Email Report]  [🗑️ Clear History]  │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Implementation Phases

### Phase 1: Complete Trip Modal (MVP)
**Estimated Time:** 4-5 hours

**Tasks:**
1. Add "Complete Trip" button to each store tab
2. Create modal/dialog with:
   - Checklist of planned items
   - Price edit fields
   - Extra items form
   - Notes textarea
   - Trip summary
3. Save trip to `shopping_history` in HuggingFace Hub
4. Option to add extra items to catalog
5. Update catalog prices if corrected
6. Clear shopping list after completion

**Files to Modify:**
- `grocery_app.py` - Add history methods
- `app.py` - Add Complete Trip UI

---

### Phase 2: History Tab
**Estimated Time:** 3-4 hours

**Tasks:**
1. Create new "History" tab in UI
2. List past 30 days of trips
3. Expandable trip details
4. Filter by store
5. Filter by month
6. Basic monthly totals

**Files to Modify:**
- `app.py` - Add History tab UI

---

### Phase 3: Reports & Analytics
**Estimated Time:** 3-4 hours

**Tasks:**
1. Monthly summary statistics
2. Planned vs actual spending breakdown
3. Store-by-store comparison
4. Top impulse purchase categories
5. Download report as text/JSON
6. Email report option

**Files to Modify:**
- `app.py` - Add report generation
- `grocery_app.py` - Add analytics methods

---

### Phase 4: History Cleanup & Maintenance
**Estimated Time:** 1-2 hours

**Tasks:**
1. Automatic cleanup of entries older than 30 days
2. Run cleanup on app startup
3. Option to export before cleanup
4. "Clear All History" button with confirmation

**Files to Modify:**
- `grocery_app.py` - Add cleanup logic

---

## 📁 New Methods Required

### In `grocery_app.py`

```python
# Phase 1: Complete Trip
def complete_shopping_trip(self, store_name, purchased_items, extra_items, notes, price_corrections):
    """Save completed trip to history"""
    
def add_extra_item_to_catalog(self, store_name, item_data):
    """Add an extra item from trip to the store catalog"""
    
def update_catalog_price(self, item_id, new_price):
    """Update catalog item price based on actual price"""

# Phase 2: History
def get_shopping_history(self, store_name=None, days=30):
    """Get shopping history, optionally filtered by store"""
    
def get_trip_details(self, trip_id):
    """Get full details of a specific trip"""

# Phase 3: Reports
def get_monthly_summary(self, year, month, store_name=None):
    """Get monthly spending summary"""
    
def get_spending_breakdown(self, year, month):
    """Get planned vs extra spending breakdown"""

# Phase 4: Cleanup
def cleanup_old_history(self, days=30):
    """Remove history entries older than specified days"""
    
def export_history(self, format='json'):
    """Export all history before cleanup"""
```

---

## ⚠️ Edge Cases to Handle

1. **Empty shopping list** - Don't allow "Complete Trip" if no items
2. **Duplicate extra items** - Warn if adding item already in catalog
3. **Invalid prices** - Validate price inputs (> 0)
4. **History storage limits** - Monitor HuggingFace storage usage
5. **Timezone handling** - Store dates in UTC, display in local
6. **Concurrent saves** - Handle debouncing with history saves

---

## 🧪 Testing Checklist

### Phase 1 Tests
- [ ] Complete trip with all items purchased
- [ ] Complete trip with some items skipped
- [ ] Add extra item (add to catalog)
- [ ] Add extra item (don't add to catalog)
- [ ] Correct a price (verify catalog updates)
- [ ] Add trip notes
- [ ] Verify shopping list clears after completion
- [ ] Verify data saves to HuggingFace Hub

### Phase 2 Tests
- [ ] View history (all stores)
- [ ] Filter by store
- [ ] Filter by month
- [ ] Expand trip details
- [ ] Verify 30-day limit works

### Phase 3 Tests
- [ ] Monthly summary accuracy
- [ ] Planned vs actual calculation
- [ ] Download report
- [ ] Email report

### Phase 4 Tests
- [ ] Auto-cleanup on startup
- [ ] Manual clear history
- [ ] Export before cleanup

---

## 📅 Timeline Estimate

| Phase | Effort | Dependencies |
|-------|--------|--------------|
| Phase 1: Complete Trip | 4-5 hours | None |
| Phase 2: History Tab | 3-4 hours | Phase 1 |
| Phase 3: Reports | 3-4 hours | Phase 2 |
| Phase 4: Cleanup | 1-2 hours | Phase 1 |
| **Total** | **11-15 hours** | |

---

## 🚀 Next Steps

1. ✅ Finalize this planning document
2. ⬜ Implement Phase 1 (Complete Trip Modal)
3. ⬜ Test Phase 1 locally
4. ⬜ Deploy Phase 1 to HuggingFace
5. ⬜ Continue with Phase 2-4

---

**Document Version:** 1.0  
**Last Updated:** 2025-11-29  
**Author:** Smart Grocery Manager Team


