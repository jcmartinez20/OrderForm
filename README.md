<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Product Order Form</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <style>
        body { font-family: Arial, sans-serif; margin: 0; background: #f8f8f8; }
        h1 { text-align: center; margin: 30px 0 20px 0; }
        .container { max-width: 1100px; margin: auto; padding: 20px; background: #fff; box-shadow: 0 2px 10px #ccc; }
        .products { display: flex; flex-wrap: wrap; gap: 28px; justify-content: center; }
        .product-card {
            border: 2px solid #333;
            border-radius: 8px;
            background: #fafbfe;
            width: 220px;
            padding: 18px 16px 20px 16px;
            text-align: center;
            box-shadow: 0 1px 6px #eee;
            position: relative;
        }
        .product-card img {
            width: 130px;
            height: 140px;
            object-fit: cover;
            border: 2px solid #1e90ff;
            border-radius: 8px;
            margin-bottom: 10px;
        }
        .qty-controls {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 5px;
            margin: 8px 0;
        }
        .qty-controls input[type=number] {
            width: 50px;
            padding: 4px;
            font-size: 16px;
            text-align: center;
        }
        .product-card button {
            margin: 2px 4px;
            padding: 4px 12px;
            font-size: 15px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        .add-btn { background: #1e90ff; color: #fff; }
        .remove-btn { background: #f44336; color: #fff; }
        .summary { margin: 35px 0 18px 0; padding: 22px; background: #e8f4fd; border-radius: 8px; }
        .summary table { width: 100%; border-collapse: collapse; margin-top: 10px; }
        .summary th, .summary td { border-bottom: 1px solid #ccc; padding: 8px; }
        .summary th { background: #f0f8ff; text-align: center; }
        .summary td.product-name { text-align: left; }
        .summary td.price,
        .summary td.qty,
        .summary td.subtotal { text-align: right; }
        .summary td.action { text-align: center; }
        .summary tfoot td { border-bottom: none; background: #e0e8ef; }
        .customer-details { margin: 33px 0 22px 0; background: #f4f4f4; border-radius: 7px; padding: 26px; }
        .customer-details label { display: block; margin: 10px 0 6px 0; }
        .customer-details input, .customer-details textarea {
            width: 100%;
            padding: 8px;
            margin-bottom: 14px;
            border-radius: 4px;
            border: 1px solid #bbb;
            font-size: 16px;
            box-sizing: border-box;
        }
        .submit-btn {
            background: #28a745;
            color: white;
            font-size: 18px;
            border: none;
            border-radius: 6px;
            padding: 12px 28px;
            cursor: pointer;
            margin-top: 18px;
            display: block;
            width: 100%;
        }
        .success-msg, .error-msg {
            margin: 0 auto 20px auto;
            padding: 12px;
            text-align: center;
            max-width: 500px;
            border-radius: 6px;
            font-weight: 600;
        }
        .success-msg { background: #d4edda; color: #155724; }
        .error-msg { background: #f8d7da; color: #721c24; }
        @media (max-width: 800px) {
            .products { flex-direction: column; align-items: center; }
            .container { padding: 10px; }
        }
        .price-label {
            font-weight: bold;
            margin-bottom: 2px;
        }
    </style>
</head>
<body>
    <h1>Order Your Favorite Products</h1>
    <div class="container">
        <div id="messages"></div>
        <form id="orderForm">
            <div class="products" id="productsContainer">
                <!-- Products will be dynamically inserted here -->
            </div>

            <div class="summary" id="orderSummary">
                <h2>Order Summary</h2>
                <table>
                    <thead>
                        <tr>
                            <th style="text-align:left;">Product</th>
                            <th>Price</th>
                            <th>Quantity</th>
                            <th>Subtotal</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody id="summaryTableBody">
                        <!-- Order summary rows here -->
                    </tbody>
                    <tfoot>
                        <tr>
                            <td colspan="3" style="text-align:right;font-weight:bold;">Total:</td>
                            <td id="orderTotal" style="font-weight:bold; text-align:right;"></td>
                            <td></td>
                        </tr>
                    </tfoot>
                </table>
            </div>

            <div class="customer-details">
                <h2>Customer Details</h2>
                <label for="customerName">Name *</label>
                <input type="text" id="customerName" name="customerName" required>

                <label for="customerPhone">Phone *</label>
                <input type="tel" id="customerPhone" name="customerPhone" pattern="[0-9+ -]{7,}" required>

                <label for="customerAddress">Address *</label>
                <textarea id="customerAddress" name="customerAddress" rows="3" required></textarea>
            </div>

            <button type="submit" class="submit-btn">Confirm and Place Order</button>
        </form>
    </div>

    <script>
    // All product data: price is from the "Price" column
    const productData = [
        {name: "Banana Chips", price: 22},
        {name: "Banana Puff", price: 68},
        {name: "Bangkok Sampaloc", price: 65},
        {name: "Belgian Butter", price: 23},
        {name: "Big Oven Brownies", price: 48},
        {name: "Biscoff", price: 250},
        {name: "Butter Cookies", price: 94},
        {name: "Butter Crunch", price: 94},
        {name: "Butter Scotch Bites", price: 83},
        {name: "Butter Stick", price: 62},
        {name: "Butcheron", price: 63},
        {name: "Cassava Chips", price: 26},
        {name: "Cheese Puff", price: 76},
        {name: "Cheese Ring", price: 78},
        {name: "Choco Crunchies", price: 66},
        {name: "Choco Iced Gem", price: 78},
        {name: "Choco Mallows", price: 66},
        {name: "Choco Stick (Big)", price: 84},
        {name: "Chocovron 12's", price: 87},
        {name: "Chocolate Chip", price: 101},
        {name: "Chocolate Wafer", price: 36},
        {name: "Cornick BBQ", price: 34},
        {name: "Cornick Cheese", price: 34},
        {name: "Cornick Sweetcorn", price: 34},
        {name: "Dansk (butter shortcake)", price: 69},
        {name: "Dilis", price: 93},
        {name: "Double chocolate Chip", price: 101},
        {name: "Egg Cracklets", price: 55},
        {name: "Fish Cracker", price: 88},
        {name: "Fishda Biscuit", price: 37},
        {name: "Garlic Bread", price: 38},
        {name: "Hiro", price: 90},
        {name: "Iced Gem", price: 78},
        {name: "Jacobina", price: 85},
        {name: "Jolly", price: 100},
        {name: "Lemmy Lemon", price: 60},
        {name: "Marie", price: 75},
        {name: "Nachos BBQ", price: 72},
        {name: "Nachos Cheese", price: 72},
        {name: "Nachos Spicy", price: 72},
        {name: "Oishi Cuttlefish", price: 40},
        {name: "Pande Toast", price: 75},
        {name: "Peanut Crunch", price: 85},
        {name: "Pusit", price: 140},
        {name: "Shingaling", price: 15},
        {name: "Square Buttermilk", price: 60},
        {name: "Stick-o", price: 80},
        {name: "Toasted Bread", price: 80},
        {name: "Uraro", price: 60},
        {name: "Vanilla Wafer", price: 60},
        {name: "Zebzeb", price: 40}
    ];

    // Exclude "Butter Cookies (round)" and "Big Ovn butterscotch"
    const filteredProductData = productData;

    // All product images mapped by name
    const productImages = {
        "Banana Chips": "https://lh3.googleusercontent.com/pw/AP1GczPTQzQZdhEaP9qyjZFpxxopPvISCEpXiEU0qhBUSyfenWh4CzTrciLcTEdqi6zB-CFZGfU6n-rKQS3kLdZT0rEus3ECI0mY_2WqCqNBCqhAhvdUTOkxZCRNb89XRTYMkRywK3g_CK3AMUpaifmvByk=w449-h599-s-no-gm?authuser=0",
        "Banana Puff": "https://lh3.googleusercontent.com/pw/AP1GczNn7zii9jIhgxmfTrjJ80sgQqZWw_k5KMG0TLNFR3Hwv1wtD0E3OprwQ9xDc7uY64Dd64DyAI8CqDSu56ykomUtT7Ibh8jR-QUepgXzuVaj4Gt3BNuPbdQ_LsxtTuOe-3QvOOQgXxTkFa2XGWGyUdY=w799-h599-s-no-gm?authuser=0",
        "Bangkok Sampaloc": "https://lh3.googleusercontent.com/pw/AP1GczPx1aRaWt43MGonoHWZI0Q5lTfzUNSYnkDKmU2dhSmENIq1ZxfmN8QE59XPyKAQ44vTzta1ypQXgnyu4D3HItHvWjWz2ZoatLNTZTGpZlpiamwPalKMPhp1V4277tc58pGujWldXlFociiJ6rxnhxM=w589-h599-s-no-gm?authuser=0",
        "Belgian Butter": "https://lh3.googleusercontent.com/pw/AP1GczOC5GbScnLj6vNzCJJScLyjqyZqYQjPMmZAge6Hs4ooie7VbT4GWSRn43Cs2iK5Rz1ZFcBaLpIddMkD5BiM3pjIvf3GF_XN4bK8ek4v65aoQQ7ZCsqpXIVePlD_jthVDI1pdnPwfQ74UhlaM6-laO4=w447-h599-s-no-gm?authuser=0",
        "Big Oven Brownies": "https://lh3.googleusercontent.com/pw/AP1GczN2ylzHwpJx6SGudcG-dEyUMTrRtLV83eokEcRpXPSeuHfuPgVPLLUXb4QKklmKw4XJ3RQ6u_XZfKeij8ie9txtL4QnmY7YIEq8JXXesybwvWzfLYWang0PAttm4u6SkSmfemAxWLZ5aaToNlems0M=w449-h599-s-no-gm?authuser=0",
        "Biscoff": "https://lh3.googleusercontent.com/pw/AP1GczN7-dJRdU5VoxIYh7XH_0T_sX1lI7av9EGkLPfQRmlevP0MG0tQOlA1Ea4JL-IWDwJpkOa1U6TCuyCksF9TIqLqiPMTs1U6Bo7tyKFWqMdt7aQ5ilgNBGFHkj0goryIWRe2a-UH8Egj8ogWMU7FtMM=w391-h599-s-no-gm?authuser=0",
        "Butter Cookies": "https://lh3.googleusercontent.com/pw/AP1GczPYXM66a_8JhXpx8Y_JhxceOjKA3n_UeloypoLBSxbYt53N_BV-jVhMWqWS9wv-FDb7sMXVjR8hCJeqaGNjpeBnYzT9doLo8VJDLt_-MQRoSM-NkDa2uYHRIZlARpEFuD9Ct42WqYiwC4NzG358zbk=w448-h564-s-no-gm?authuser=0",
        "Butter Crunch": "https://lh3.googleusercontent.com/pw/AP1GczPg9QRtv-Ygs3N7_Q1G9EWv-7MKUuh3gXb4ATb8lQh0Y2wRhepBPfap1UN7qUgsyO6eDW3Irn-7clOfGErQELucwDinz6A02llnbuZm7_PV4ApoPSjvf8MJ8vVvAEaI-R5_JzZFPfYMWgS00HLaUrU=w449-h599-s-no-gm?authuser=0",
        "Butter Scotch Bites": "https://lh3.googleusercontent.com/pw/AP1GczNvMHS49auKmIQtGXH4IdDoAZhl1hVfKuChPVhGM2gFBDlhHyN2UiG_PedU3qvUCeg9_09DB2_NHF8okkKfMlVIQC7RG6TFU8FpL4a3yEMO-Pp9W15FWx17fa1SMBoLxtFrA80J3egs18d3Y2Wi2lM=w489-h599-s-no-gm?authuser=0",
        "Butter Stick": "https://lh3.googleusercontent.com/pw/AP1GczOFhvDNpK5gMsZRX1AkQOejn2dRDqmn7PtD0mUzPCWmr5Fh8VzK0uL7Di-SRN_7E_Vg_0yWuIMXQG0nJbzpLsAWcVip3s-d_RyWUY3cWqcGinaGlmbbXL8PPPNwQYcuy7nagylC_PlognxoEHZO_vc=w480-h599-s-no-gm?authuser=0",
        "Butcheron": "https://lh3.googleusercontent.com/pw/AP1GczMol-dykOTn_FRitB2O-OqDFLn-R4E_U0S4CcKQ7l-XggdZTL1fj8gcRC3ZfZ11SneaPZSnohXf29TeOwPX0a4LxJ_SuvlSfWGWR0uFXUP7HsX5x0mfV41GqV-2dLPi0up2ZVGY4ciPwLww-S50oXs=w438-h599-s-no-gm?authuser=0",
        "Cassava Chips": "https://lh3.googleusercontent.com/pw/AP1GczOqteObClnkY0Cb-JVo04w-FcWYsM_y7xGbz_I2PNXot8lqq-5f-km7liuciOUTD5b51aO1oJn7fgYZEHGtmRh_x0x7jtq8p-MbYodB4kcF_qvPB7WpjZfU-VNdCXli6JzH3K3S1SWsoclUCVhNlkU=w562-h749-s-no-gm?authuser=0",
        "Cheese Puff": "https://lh3.googleusercontent.com/pw/AP1GczOLOzZ9nqETIZfxZkENwFFDJPATZj-IdqdMc_szlRL1MG3v7skQl-VIjQRrOKnjk_CumR3nnknLfkyO2zlAQBK4dD2K1Prkpqg6k83SXmDywdYUIRUts9qbbd3QCHFtTOJU6VPXHCMZNDrwBrEsslk=w449-h599-s-no-gm?authuser=0",
        "Cheese Ring": "https://lh3.googleusercontent.com/pw/AP1GczOWY8Pecv14PJbaFTJO0uogs6uIsiTEng7hN-ODPnXsLpllQUmKRGkukmduOLiUZKRDWtW5hGjgj3wb7Bg-lT5qRYYpnzWI96LAFyq75jI-Gch_bBfgApDPl2Lx4Nb1-nT2wUV-gnxEq02H97NT9-Q=w449-h599-s-no-gm?authuser=0",
        "Choco Crunchies": "https://lh3.googleusercontent.com/pw/AP1GczNNMoKfpkDfEU7BIaeokP6sKxaFvYFD8cmh4bWa0_PlDbpCQ0VYuj-yD_8sXv0dCk6VRMbIQMrftXzd6hjUlTsa4rcr3yxQm_T_v8GxYsmI0IMi-IQZ2FYwuFUvbDIhnebtl-F1UHXjVNWcJ_FOh8o=w633-h564-s-no-gm?authuser=0",
        "Choco Iced Gem": "https://lh3.googleusercontent.com/pw/AP1GczPkgwb7NSilmIc4fP1VPkN7T7j3m8eRR-suBi4_DA95bCQT7f-Z0VLdn_BOIt166cpyQH39b892Eg98PI6h06pdv90bcN_yEz0BgLPIuTv_zGU77AZkb6BUC2ySuDGSFZjxuT5sjSemGfje3BKRIV8=w449-h599-s-no-gm?authuser=0",
        "Choco Mallows": "https://lh3.googleusercontent.com/pw/AP1GczNYVohnRO_mSKaDDfucIRetrcJYvgNbnzkW2Nay0CWSQ2ePqZCLRnfX1OX85bXzHFi9p1cE-CPZm2qVgrKfK51gwZ1_SB_zyP5khlYyiiyCFFf_qYaVwqNx4worvOOd4snYF6RO-_c99T3nCVCW2aA=w740-h599-s-no-gm?authuser=0",
        "Choco Stick (Big)": "https://lh3.googleusercontent.com/pw/AP1GczNYVohnRO_mSKaDDfucIRetrcJYvgNbnzkW2Nay0CWSQ2ePqZCLRnfX1OX85bXzHFi9p1cE-CPZm2qVgrKfK51gwZ1_SB_zyP5khlYyiiyCFFf_qYaVwqNx4worvOOd4snYF6RO-_c99T3nCVCW2aA=w740-h599-s-no-gm?authuser=0",
        "Chocovron 12's": "https://lh3.googleusercontent.com/pw/AP1GczPX7Tm43f0JTKTv3I_huHK4hyxX42Bb24WD4rJRUsOWkG0mTvF46rRgamDGf160LF_NQxcoWcw3m37s8ytb_UzrUFjoWo59C0Vhrp07dcAYowWorA657vgoMPxCqd62kV6Q3QL7r3tnEM0h0li9JJ8=w913-h599-s-no-gm?authuser=0",
        "Chocolate Chip": "https://lh3.googleusercontent.com/pw/AP1GczOVplVr44eyOvJH8dGuIKIrj9WUQShfiFPrAVaAuAuBYx_MTLBODU20ZW3PaDCfAMkl5xg6d4jCzH-JGshTDOi4B4vPEus9MSFX0vkMkUz_aPzdW8_gOCU4-x6R5V_xko5CzKiSFto8tvn5raf_di0=w538-h545-s-no-gm?authuser=0",
        "Chocolate Wafer": "https://lh3.googleusercontent.com/pw/AP1GczN7iClM9gVMsTGUfdivky6s0CuO8F8L-pelqgYBH8dCH0_Aggx5oKG8efBj3b2kXxe0ZsrC90Ct9BF2ZkCczUd4NHOWm3GIEgcoCGe9iV9fdacZCtJW1NENgAbXDCdW2AbOoH6b2mKlMOGgXkOH7xQ=w505-h599-s-no-gm?authuser=0",
        "Cornick BBQ": "https://lh3.googleusercontent.com/pw/AP1GczPqGQMTT2iCaelns4rJDZFpFoXgLgzuxNeDm2QPOPwtStt49uuIBbrz1H6b78FOg9tnWx4IHYZggzZiz_AxQwcwxJxPy42o26NiihXMm76shojVX-blA05Kyh-F--QSy0JIpjFd6djVyzyxy15LJeY=w556-h599-s-no-gm?authuser=0",
        "Cornick Cheese": "https://lh3.googleusercontent.com/pw/AP1GczN88zj93e6EAcnvJZ4i1YeC3E421NBHipg2eteGO-T6lny7rhN603I1myoCRWAQqWYGLoLinM7xlOgThA1Emwe-pfouRy3tLKM-mx3qF9mmNLT_2fRYaU-U8AzgsXBkfkpuNTJd_h0GjV-H5Adu7RQ=w449-h599-s-no-gm?authuser=0",
        "Cornick Sweetcorn": "https://lh3.googleusercontent.com/pw/AP1GczPeztMBYhU8IURsLPxiv-K5F-Sfr--jq8B7XkIQxZ8nhQYRy7llJyFm4Mcb_n-99LccSIihiWKHLRKxX2GSe5LU8-YG7L3z4oseBD6RSBpjlSIZGLD46ugaetxE-yPBKWm-UDch_iXjNZQheuqIQrA=w496-h599-s-no-gm?authuser=0",
        "Dansk (butter shortcake)": "https://lh3.googleusercontent.com/pw/AP1GczPU6PjUEnSJSGBTe-JnQKVjbdtxBxCYgr0wdfJS1J_HN9wilXv0MwlLeQw4sllnnuqpF7krGhCcBI7TVTDGPFnRwXzkV3MQfi0E7g1D_SpyJ0bev29g1ge5VpiaXS1i88UkVsWUj0yllxhNxJk2pKI=w449-h599-s-no-gm?authuser=0",
        "Dilis": "https://lh3.googleusercontent.com/pw/AP1GczNwoZddReJJeVTqdB0JZ0UBXgjqIeTgGaWZSzWd9Ef7XJonom2Co05ISXXypY2TM3Ab8zC5qau0aLaKKiXmz0exN8yCvn6yCZhkghpnvlpi7bwajKOLify59h6MILHONw0q93Lc2ZSC1ACth3DzNaA=w404-h599-s-no-gm?authuser=0",
        "Double chocolate Chip": "https://lh3.googleusercontent.com/pw/AP1GczMi0e8-5_UpDF7ZJUU42LtZdtmGYbbkjlN0J3l8XXvXd1M9NT7UysAgdDRCE-6hfux9uPcuON6BNMtNVzQmuS6GbVCcU_S7Rh2bh3e1cjtl0naAODP7GLBNts20JELhMBixcTDC2rUuxCX3BlnZ53M=w449-h599-s-no-gm?authuser=0",
        "Egg Cracklets": "https://lh3.googleusercontent.com/pw/AP1GczORL520ZS5QbB77hrdvELL1pfrJkVVpiuUiZXNTwBWfcz1FLNpliFG8_h-mPbSb1i6j3S0wdTjYI2RBUNcGC0nW3RrFENn0OBJLeZZPi8OmEtWAsteRA8tY7V1ODvpmmE-TS_XIkzBjirYtC-MPyFU=w449-h599-s-no-gm?authuser=0",
        "Fish Cracker": "https://lh3.googleusercontent.com/pw/AP1GczN-TV6l9wxm3aLQpuc_uCZtEADP0esPkYj-dKO-L3OJSSSdgD65-GQhRiHrc-8130vn8sqtjTG-IpnPvkI4WMnuyDCEhxnh3uddDeniOEZyLkTvm43VAVm_qPMZGQyIbSiIWDEg-9RqkrtkQLmpSjc=w449-h599-s-no-gm?authuser=0",
        "Fishda Biscuit": "https://lh3.googleusercontent.com/pw/AP1GczNwoZddReJJeVTqdB0JZ0UBXgjqIeTgGaWZSzWd9Ef7XJonom2Co05ISXXypY2TM3Ab8zC5qau0aLaKKiXmz0exN8yCvn6yCZhkghpnvlpi7bwajKOLify59h6MILHONw0q93Lc2ZSC1ACth3DzNaA=w404-h599-s-no-gm?authuser=0",
        "Garlic Bread": "https://lh3.googleusercontent.com/pw/AP1GczNBAK4-ZAOC5B4W_dYeFDYRRpp3DHAmmi63oPzn_4-levgdWi3jfEUCnVCSYMr1EIqe_ayyRcRV4FsIrloNYw-K1AYIv4DhpBWAtYJlo9crWSpLvWEYbsQZujWUvxJynFDVrrjrKedfF64Ete7jz_8=w594-h599-s-no-gm?authuser=0",
        "Hiro": "https://lh3.googleusercontent.com/pw/AP1GczPqjYtwGhGSxEk_8mPGhBGIeKyoWEA9Kl7sWxEG2a28Uuu0gTKGXC1jydKi5VOi7RIGlLLY1jXA9agOXyAJIA-nVbfiBoTFixToVPIvth8Ot0jqmzN_7Xjm6CNpy5Iirsql3g1qXF8mOYfGx-x7P4g=w799-h599-s-no-gm?authuser=0",
        "Iced Gem": "https://lh3.googleusercontent.com/pw/AP1GczO1fkqGdHAFGW1ER1pMYbMIVNNKtsEatBcRb3T0c0qtaIo_4JHmni3bXEFFr5Ez0jClYI_XYpbHQjBCGaqjkJw1N_c61n0_vY53X2U6to684hajXo1z5oh5151FUNkqktrBLQwTUHrsxOheFoErsr4=w449-h599-s-no-gm?authuser=0",
        "Jacobina": "https://lh3.googleusercontent.com/pw/AP1GczNBDBXDxXLuBl4LP9_X4G72HVGls6o_3lwL7YXo68q15_ySo8Yo7cYBWzJ4OcKO_hvy1-cttxIE0IIHnGPp32AJalsZMS3jHub4qIAGrQWU-aYKLIck0_bpak2f1JAml7E5E-G7DHl32KM9ju1LxLc=w403-h599-s-no-gm?authuser=0",
        "Jolly": "https://lh3.googleusercontent.com/pw/AP1GczNYKrFlLbngvRxcVbMKQXJSptaXy9SuuRos1KArNh0WGlrMsALyxni9bX13uKDZmFOEtk3rvULoAyvhuq3QI92i4JiZCH5O8qPrkNjmU5WmmF31R_ZcO5nTaeUaHnW6oBIY64Bt9bDEwhfVWv45K6E=w849-h599-s-no-gm?authuser=0",
        "Lemmy Lemon": "https://lh3.googleusercontent.com/pw/AP1GczPzkBIwEtU7Ys4yqRnnjg-Eu0isAzWH5KFN5OgsOppId_v7sIMpU3i_YJFvhUJ32TuvQIeMx5j36VxcexPOo3OHvGM9VTJlIfmgbpUSDRllKBquj2NPz1RP2EjbOiEotSUIJFILTHEpqfOPczNAP68=w472-h599-s-no-gm?authuser=0",
        "Marie": "https://via.placeholder.com/130x140?text=No+Image",
        "Nachos BBQ": "https://lh3.googleusercontent.com/pw/AP1GczOjj9JLMnN4lBlDNgZGsg-RyEJ-NmXCoT81YOZ9nlmBlYNMILYK5L69zs_76-vCd9RH6YP_YoKwDpVgXvhkxUrB_wyzedzPjKdkZEt0E2wI1WEMYhKt86yqGipjqUT2dX0JCoewiDwEXuHUQNY1wT4=w475-h599-s-no-gm?authuser=0",
        "Nachos Cheese": "https://lh3.googleusercontent.com/pw/AP1GczPZks7FP76uRrvBmdBULx2PSeAjMZA9mjtFqZnJMi_JOkyOhTAzDdrhgL8FWkWfqAzB1zUbB3Y2iI1GTUDEMdjuYIBTU3TeLlfLzWiBnpuZ1vvUCiZGm5De9RVN_UQFSNRLUrJoFYEH7450Ck_L0qo=w449-h599-s-no-gm?authuser=0",
        "Nachos Spicy": "https://lh3.googleusercontent.com/pw/AP1GczP5MO9WNKhjzdaNleEvzDXmU4E2XaWgYoCExwUr7Y2RfdVVblDFa8VGLjVTLmwi0B0MjALkzO1IkWgT04gNWkxZ9V5g6ryUiU_vPuvt8ltBwO48KAMCcpCaO1Da75VRK_3KxUNxdbxgsaCKOWHwjDs=w449-h599-s-no-gm?authuser=0",
        "Oishi Cuttlefish": "https://lh3.googleusercontent.com/pw/AP1GczPNDc_BHj_QKEp05vrf0dRwB1TJy6bisDqfsc9-Y4Kh2jTUJ5i8umOYRqZfLQ_GgRWHmlk7VTdH0_qEZvupipShu2FQbbOxw7-A65JwFibucLJSxEdsLfcr0Ph3lLChMYndzc4ABM4oJW7yq4IIj5I=w449-h599-s-no-gm?authuser=0",
        "Pande Toast": "https://lh3.googleusercontent.com/pw/AP1GczOAJ56nQRkNGOvJP9xUi6XV_gBecUYBwRVuhBlGYPMGi3oiwk0piPIA6iNenHiEDVbsMbpaZwtQq07Q2YTI8ks36IXloQAHkUOTYWqqseDxFmtaownqRbF0LrQ1SVbeK-MNOhbaNzFieqURYV6q50M=w449-h599-s-no-gm?authuser=0",
        "Peanut Crunch": "https://lh3.googleusercontent.com/pw/AP1GczMkDcR-izPGYVQMXbv3PQNe81yVgwkwVSinlxSneIAEAga2selJAIAwNcLxuk0z2TlFUkHKTZ4cETQFnAgJcbzpC_Auxd7xecseCznY0aYgisujRKjPTrCapnbUaLgjuuUTRa2CU1tRkX54lEo5ft4=w449-h599-s-no-gm?authuser=0",
        "Pusit": "https://lh3.googleusercontent.com/pw/AP1GczOPry3XfUMmDH2kzvFfO2jBjb3DNRT9VMGF1ebAKA3iOmgjKO9gVbwjP6bE8EDwp_WxE2vyz0F2AAe6Wuc8hoxk-GJ9qZAi2-W1gLQUDcbtuapt7FQEKlhhoZ0tGEz-LVRsOdONyIdCjmUrwZK9Lpw=w521-h599-s-no-gm?authuser=0",
        "Shingaling": "https://lh3.googleusercontent.com/pw/AP1GczMz_kfI82PPeyvFL90WSN8_rLuu2AljDJtUdzBp18E81A2HiJHroXDyJ6iAiZOx6CA7VHzgihC0PR53LFpuB4d_pcDI2TPA9gE7MmX87aZTVSzEmZii7__VmQx5vPlIq3kuC0vG5PlS2k1XpDmqoDA=w680-h599-s-no-gm?authuser=0",
        "Square Buttermilk": "https://lh3.googleusercontent.com/pw/AP1GczMyAMpAMmAK7ly-cwHWetfnxMxYwS0nGgjQ65jJ9q7S2p3pzX1qjmUC1a2I1rp-e-6Odz0HhyiD6ic0UduuGDC1IiJro9Rss4g7f0DeJc2srtpwTcPHslW-k5NFekW2O0xnn55giKK5L2f7Y7q67Is=w387-h599-s-no-gm?authuser=0",
        "Stick-o": "https://lh3.googleusercontent.com/pw/AP1GczMjSu5AEnERKao-0wvT7qLogiKLvJy5eoBcxz_DLIrj3IjTE6lIpFeV1bjzUp3_J2DaQoPCbGpT2vw_4ZPYvXqTNvJfuUCPL_-JbNtFjjLpuNxW1HJ-THId_0IDUAaHqXsDN2ho43kTxnmpRzYBl_k=w400-h599-s-no-gm?authuser=0",
        "Toasted Bread": "https://lh3.googleusercontent.com/pw/AP1GczMxacyrm20C9j0zibxStObi4ynIyLgOf_8qkozxIgdaHxtvc8xhFQ6F949UD_FDcxcyyAXQi5sVpw1Wi4loMwwe9rwKe7sNQYh02FjuDYTjcgNE0cqdKNHrHewQrR74kyiFqADIo4zqAGR7FRNi1hQ=w449-h599-s-no-gm?authuser=0",
        "Uraro": "https://lh3.googleusercontent.com/pw/AP1GczP1jc_xVqHG4pwPScVaCoZBOIGTbGGCzM9iXTYRLZcXmuBVNEWY3a3ah9nMaA9aogJzrRyEFxZeWgQBiJylI-eYjdgeHHxio6eRl-Xu1dwLw7kR6INMfUrlunwDlFrpq4CJ1sKHLXCNAUz3h-SKa1M=w409-h599-s-no-gm?authuser=0",
        "Vanilla Wafer": "https://lh3.googleusercontent.com/pw/AP1GczNfcaSmscGTjjcjZBACAvPZEGVX3eml61EwmzE6xri1j-oeBNUqqifU1sUZInz72Vzn8bgQPBpkCgwMW3dbaWo3hXidjNoFh0af5UxE1hjx0hhwDKqAllZhUI3APKB6zmpcgxyYGBjJWMeOO2HUN70=w555-h599-s-no-gm?authuser=0",
        "Zebzeb": "https://lh3.googleusercontent.com/pw/AP1GczMeXWYchDr2saqbDQARYl06iPJeG9XGoAJxHlbfVWqFxaBGvdUdBAoSMjW1V_CWdcB3H6XtyD3K5EUwbrkV3qUUGhlaAcAU77GXCeph4vKYZfz-z0QFUh83RxdoxbaX1NLlPwceqS9xcPSU6wWlU0Q=w449-h599-s-no-gm?authuser=0"
    };

    // Merge productData with images and initial qty 0
    const products = filteredProductData.map(p => ({
        ...p,
        img: productImages[p.name] || "https://via.placeholder.com/130x140?text=No+Image",
        qty: 0
    }));

    // In-memory order: product name as key, value is quantity
    let order = {};

    function renderProducts() {
        const productsDiv = document.getElementById("productsContainer");
        productsDiv.innerHTML = "";
        products.forEach((product, idx) => {
            const value = order[product.name] || 0;
            const div = document.createElement("div");
            div.className = "product-card";
            div.innerHTML = `
                <img src="${product.img}" alt="${product.name}">
                <div style="font-weight:bold; margin-bottom: 5px;">${product.name}</div>
                <div class="price-label">₱${product.price}</div>
                <div class="qty-controls">
                    <button type="button" class="remove-btn" onclick="changeQty('${product.name}', -1)">-</button>
                    <input type="number" id="qty_${idx}" min="0" value="${value}" onchange="setQty('${product.name}', this.value)">
                    <button type="button" class="add-btn" onclick="changeQty('${product.name}', 1)">+</button>
                </div>
            `;
            productsDiv.appendChild(div);
        });
    }

    window.changeQty = function(productName, step) {
        order[productName] = Math.max(0, (order[productName] || 0) + step);
        renderProducts();
        renderSummary();
    };

    window.setQty = function(productName, val) {
        let v = parseInt(val, 10);
        if (isNaN(v) || v < 0) v = 0;
        order[productName] = v;
        renderProducts();
        renderSummary();
    };

    window.removeFromOrder = function(productName) {
        delete order[productName];
        renderProducts();
        renderSummary();
    };

    function renderSummary() {
        const tbody = document.getElementById("summaryTableBody");
        tbody.innerHTML = "";
        let total = 0;
        Object.keys(order).forEach(name => {
            const qty = order[name];
            if (qty > 0) {
                const product = products.find(p => p.name === name);
                const price = product ? product.price : 0;
                const subtotal = qty * price;
                total += subtotal;
                const tr = document.createElement("tr");
                tr.innerHTML = `
                    <td class="product-name">${name}</td>
                    <td class="price">₱${price}</td>
                    <td class="qty">${qty}</td>
                    <td class="subtotal">₱${subtotal}</td>
                    <td class="action"><button type="button" class="remove-btn" onclick="removeFromOrder('${name}')">Remove</button></td>
                `;
                tbody.appendChild(tr);
            }
        });
        document.getElementById("orderTotal").textContent = `₱${total}`;
        if (tbody.innerHTML === "") {
            tbody.innerHTML = `<tr><td colspan="5" style="text-align:center;color:#999;">No items selected.</td></tr>`;
            document.getElementById("orderTotal").textContent = "₱0";
        }
    }

    document.getElementById("orderForm").onsubmit = function(e) {
        e.preventDefault();
        document.getElementById("messages").innerHTML = "";

        if (Object.keys(order).filter(k => order[k] > 0).length === 0) {
            showMsg("Please confirm at least one product.", false);
            return;
        }
        const name = document.getElementById("customerName").value.trim();
        const phone = document.getElementById("customerPhone").value.trim();
        const address = document.getElementById("customerAddress").value.trim();
        if (!name || !phone || !address) {
            showMsg("Please fill all customer details.", false);
            return;
        }

        let orderItems = Object.entries(order)
            .filter(([_, qty]) => qty > 0)
            .map(([k, v]) => `${k}: ${v}`)
            .join(", ");
        let formData = {
            "Order": orderItems,
            "Name": name,
            "Phone": phone,
            "Address": address
        };

        // Update this to your own deployed Google Apps Script URL:
        const GOOGLE_SCRIPT_URL = "https://script.google.com/macros/s/AKfycbyDsGDSHX6DPXSMwAWntq0YjNdp4sPwLDuW50wo_VA9MB-PyHMXV4m-HNDNzlVXSEA6mw/exec";

        fetch(GOOGLE_SCRIPT_URL, {
            method: "POST",
            mode: "no-cors",
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify(formData)
        }).then(() => {
            showMsg("Order submitted successfully! Thank you for your order.", true);
            order = {};
            renderSummary();
            renderProducts();
            document.getElementById("orderForm").reset();
        }).catch(() => {
            showMsg("Order failed to submit. Please try again later.", false);
        });
    };

    function showMsg(msg, success) {
        document.getElementById("messages").innerHTML = `<div class="${success ? "success-msg" : "error-msg"}">${msg}</div>`;
        setTimeout(() => {
            document.getElementById("messages").innerHTML = "";
        }, 6000);
    }

    renderProducts();
    renderSummary();
    </script>
</body>
</html>
